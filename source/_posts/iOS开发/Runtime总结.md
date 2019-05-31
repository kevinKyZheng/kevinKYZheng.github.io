## Runtime常用API
* *获取*和*添加*类的,属性,方法,实例变量,协议
```
class_copy,ivar,property,method,protocol
class_add
```
* 设置关联对象
```
objc_setAssociatedObject
objc_getAssociatedObject
```
* 获取变量名,变量类型
```
ivar_getName
ivar_getTypeEncoding
```

* 获取属性名和属性真实名称
```
property_getName
property_getAttributes
```

## Runtime应用
* 动态交换方法实现
```
SEL swizzledSelector = @selector(xxxviewDidLoad);
Method swizzledMethod = class_getInstanceMethod(class, swizzledSelector);
method_exchangeImplementations(originalMethod, swizzledMethod);
```
* 动态添加对象的成员变量和方法
* 获取类的所有成员变量和方法
    - 动态变量控制(先遍历所有变量,然后找到对应的变量)
    - 自动归解档
    - 字典转模型
    以上都是,class_copyIvarList或者class_copyPropertyList,先取出所有属性或者变量然后执行其他操作


## 面试相关
1. 简述runtime
OC底层的一套C语言的API,编译器最终将OC代码转化为运行时代码.
OC的对象,其实是C中指向结构体的指针,方法是C中的函数,OC中调用方法都会变成向对象发送消息.OC的方法只要声明了就可以调用,可以不用实现,警告不会报错.
2. 根据sel找到imp实现
```
class_getInstanceMethod(class, @selector())
method_getImplementation
```
3. 消息机制
* 先转化为objc_msgsend方法,通过isa,superclass指针在当前类和父类中查找;
* 如果没有,进行动态方法解析,通过如下两个方法,返回YES继续往下执行
```
+ (BOOL)resolveClassMethod:(SEL)sel
+(BOOL)resolveInstanceMethod:(SEL)sel
```
* 消息转发
```
派发给别的对象,返回值是响应消息的对象,nil则下一步
- (id)forwardingTargetForSelector:(SEL)aSelector
返回值不是nil,执行forwardInvocation
- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector
- (void)forwardInvocation:(NSInvocation *)anInvocation
```
* 发送消息函数
    - objc_msgSend
    - objc_msgSend_stret
    - objc_msgSendSuper
    - objc_msgSendSuper_stre
     消息发送给超类,调super;返回值时数据结构不是简单值,用stret
4. 平时使用
* 关联对象,给分类添加属性
* 交换方法实现,
* 遍历所有的变量,实现自动归档解档,字典转模型
5. 为什么要动态添加方法
一个类的方法特别多的时候,加载类到内存会特别好资源.懒加载没用,一个类只要实现了某个方法就会被加进内存
6. weak属性的实现

## 反射

运行时选择创建哪个实例,并动态调用方法
```
self.isKind(of: AnyClass)
self.isMember(of: <#T##AnyClass#>)
self.responds(to: <#T##Selector!#>)
NSClassFromString(<#T##aClassName: String##String#>)
```
例如根据字符串创建类,方法等,或者在运行时做一些判断,如上:是否能响应某个信息,是否是某个类的实例.

