动态交换方法实现
动态添加对象的成员变量和方法
获取类的所有成员变量和方法
6. 方法汇总

```
//获取属性,方法,实例变量
class_copy,ivar,property,method
//添加
class_add 同上

//分类添加属性
objc_setAssociatedObject
objc_getAssociatedObject

//获取变量名,变量类型
ivar_getName
ivar_getTypeEncoding

//获取属性名和属性真实名称
property_getName
property_getAttributes
```

1. 消息转发


```
//在当前类中寻找实现
+(BOOL)resolveInstanceMethod:(SEL)sel{
//    if(sel == @selector(foo:)){
//        class_addMethod([self class], sel, (IMP)foodMethod, "v@:");
//        return YES;
//    }
//    return [super resolveInstanceMethod:sel];
}
//派发给别的类
- (id)forwardingTargetForSelector:(SEL)aSelector{
//    if(aSelector == @selector(foo)){
//        return [test new];
//    }
//
//    return [super forwardingTargetForSelector:aSelector];
    return nil;
}

- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector{
    if([NSStringFromSelector(aSelector) isEqualToString:@"foo"]){
        return [NSMethodSignature signatureWithObjCTypes:
                "v@:"];
    }
    return [super methodSignatureForSelector:aSelector];
}

- (void)forwardInvocation:(NSInvocation *)anInvocation{
    SEL sel = anInvocation.selector;
    
    test * t = [test new];
    if([t respondsToSelector:sel]){
        [anInvocation invokeWithTarget:t];
    }else{
        [self doesNotRecognizeSelector:sel];
    }
}
```

2. 为分类添加属性
```
//关联对象
void objc_setAssociatedObject(id object, const void *key, id value, objc_AssociationPolicy policy)
//获取关联的对象
id objc_getAssociatedObject(id object, const void *key)
//移除关联的对象
void objc_removeAssociatedObjects(id object)
```

3.方法替换
```
//获取实例对象方法
class_getInstanceMethod
class_addMethod
class_replaceMethod
//
method_exchangeImplementations
```

4.自动归解档
```
///获取达到的所有属性
Ivar *class_copyIvarList(Class cls , unsigned int *outCount)
//获取成员变量名
ivar_getName(Ivar v)
//获取成员变量类型
const char *ivar_getTypeEndcoding(Ivar v)
```

5.字典转模型
```
class_copyPropertyList
property_getName(property)
```

