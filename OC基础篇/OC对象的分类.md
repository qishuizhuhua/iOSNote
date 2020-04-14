####OC对象的分类

* instance对象（实例对象）
* class对象（类对象）
* meta-class对象（元类对象）

##### instance

instance对象是同坐alloc出来的对象，每次调用alloc都会产生新的instance对象

instance对象在内存中存储信息包括

isa指针

其他成员变量

##### class

```
Class objectClass1 = [object1 class];
//或者
Class objectClass2 = object_getClass(object1);
```

同一个对象，每个内存中有且只有一个class对象

class对象在内存中存储信息包括

isa指针

superclass指针

类的属性信息（@property）类的对象方法信息（instance method）

类的协议信息（protocol）类的成员变量信息【描述信息】（ivar）

##### meta-class

```
Class objectMetaClass = object_getClass([NSObject class]);
//是否为元类对象
class_isMetaclass();
```

meta-class对象和class对象的内存结构一样。但用途不一样。

内存中存储信息主要包括：

isa

superclass指针

类的类方法信息（class method）

。。。

object_getClass()和objc_getClass()，class区别

这里看Runtime源码

object_getClass()

传入instance，class，meta-class

返回class，meta-class，NSObject基类的meta-class对象

objc_getClass()

传入字符串类名，返回类对象

class

返回类对象

