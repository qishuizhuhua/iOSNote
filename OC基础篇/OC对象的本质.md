* OC代码底层实现是C/C++

OC -> C\C++ -> 汇编 -> 机器语言

* OC面向对象都是基于c\c++的数据结构实现的
* oc的对象、类基于c\c++的（ 结构体） 实现
* OC转C\C++

xcrun -sdk phones clang -arch arm64 -rewrite-objc OC源文件 -o 输出的CPP文件

这里分析oc对象本质，通过转C\C++代码来查看

如果要验证，则通过重新结构体。

* NSObject的底层实现

```
@interface NSObject {
	Class isa; 
}

//实现  
struct NSObject_IMPL {
	Class isa;  //64bit 8字节  32bit 4字节（这是代码分析）
}

//这里实际占用是16字节

//获得NSObject类的实例对象成员变量所占用的大小
class_getInstanceSize([NSObject class])  //8byte
//获得obj指针所指向内存的大小
malloc_size((__bridge const void *)obj); //16byte
```

OC源码地址：https://opensource.apple.com/tarballs/

* 一个NSObject对象

系统分配了16byte给NSObject对象（malloc_size获得）

在内部实际只使用了8个字节（class_getInstanceSize获得，64bit）

穿插技巧：

xcode查看内存

Dubug -> Dubug workflow -> viewMemory

常用LLDB指令

print、p、po 打印对象

读取内存

memory read/数量格式 字节数 内存地址

简写：x/数量格式字节数 内存地址

eg：x/3xw 0x10010

格式：x 16进制，  f 浮点， d 10进制

字节大小： b byte 1字节， h half word 2字节，w word 4字节， g giant word 8字节

* 有成员变量的继承类

```
@interface Student : NSObject {
	int _no;
	int _age;
}

//实现  
struct Student_IMPL {
	Struct NSObject_IMPL NSObject_IVARS; //isa
	int _no;
	int _age;
}
```

* 复杂的继承结构

```
@interface Person : NSObject {
	int _age;
}

//实现  
struct Person_IMPL {
	Struct NSObject_IMPL NSObject_IVARS; //isa
	
	int _age;
}

@interface Student : Person {
	int _no;
}

//实现  
struct Student_IMPL {
	Struct Person_IMPL NSObject_IVARS; //isa
	int _no;
}
这里有内存对齐概念，即不足16字节补齐，所以只会有8的倍数
```





