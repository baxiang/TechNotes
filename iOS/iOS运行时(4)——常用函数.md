###1 类
####1.1 创建对象
```
 id class_createInstance(Class cls, size_t extraBytes)
```
eg:
```
size_t size = class_getInstanceSize([Person class]);
Person *person = class_createInstance([Person class], size);
```
####1.2 获取类名
```
const char *class_getName(Class cls) 
```
eg
```
const char *name = class_getName([Person class])
```
####1.3 获取父类
```
Class class_getSuperclass(Class cls)
```
eg
```
Class class = class_getSuperclass([Person class]);
```
####1.4 获取类的大小
```
size_t class_getInstanceSize(Class cls)
```
### 2 成员变量
####2.1获取成员列表
```
Ivar *class_copyIvarList(Class cls, unsigned int *outCount)
```
eg:
```
  unsigned int count;
  Ivar *ivarList = class_copyIvarList([self class], &count);
  for (int i = 0; i < count; i++) {
      Ivar ivar = ivarList[i];
      // 获取成员属性名
      NSString *name = [NSString stringWithUTF8String:ivar_getName(ivar)];
      NSString *type = [NSString stringWithUTF8String:ivar_getTypeEncoding(ivar)];
      NSLog(@"%@%@", type, name);
  }
```
###3 属性
####3.1 获取成员列表
```
objc_property_t *class_copyPropertyList(Class cls, unsigned int *outCount)
```

eg:
```
  unsigned int count;
  objc_property_t *propertyList = class_copyPropertyList([self class], &count);
  for (int i = 0; i < count; i++) {
      objc_property_t property = propertyList[i];
      // 获取成员属性名
      NSString *name = [NSString stringWithUTF8String:property_getName(property)];
      NSString *type = [NSString stringWithUTF8String:property_getAttributes(property)];
      NSLog(@"%@%@", type, name);
  }
```
### 4 方法
####4.1 获取实例对象的方法
```
Method class_getInstanceMethod(Class cls, SEL name)
```
eg
```
Method method = class_getInstanceMethod([self class], @selector(methodName));
```
####4.2 获取类的方法
```
Method class_getClassMethod(Class cls, SEL name)
```
eg
```
Method method = class_getClassMethod([self class], @selector(methodName));
```
####4.3 获取方法的函数指针(IMP)
```
IMP class_getMethodImplementation(Class cls, SEL name)
```
eg:
```
IMP imp = class_getMethodImplementation([self class], @selector(methodName));
```
####4.4获取方法列表
```
Method *class_copyMethodList(Class cls, unsigned int *outCount)
```
```
 unsigned int count;
  Method *methodList = class_copyMethodList([self class], &count);
  for (int i = 0; i < count; i++) {
      Method method = methodList[i];
      NSLog(@"%s%s", __func__, sel_getName(method_getName(method)));
  }
```
####4.5增加方法
```
BOOL class_addMethod(Class cls, SEL name, IMP imp, const char *types)
```
```
 class_addMethod(theClass, selector,  method_getImplementation(method),  method_getTypeEncoding(method));
```
####4.6方法替换
```
IMP class_replaceMethod(Class cls, SEL name, IMP imp, 
                                    const char *types) 
```
```
 class_replaceMethod(class, swizzledSelector, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod));
```
#### 4.7 方法交换
```
+ (void)load {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        Class class = [self class];

        SEL originalSelector = @selector(viewWillAppear:);
        SEL swizzledSelector = @selector(bx_viewWillAppear:);

        Method originalMethod = class_getInstanceMethod(class, originalSelector);
        Method swizzledMethod = class_getInstanceMethod(class, swizzledSelector);

        BOOL success = class_addMethod(class, originalSelector, method_getImplementation(swizzledMethod), method_getTypeEncoding(swizzledMethod));
        if (success) {
            class_replaceMethod(class, swizzledSelector, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod));
        } else {
            method_exchangeImplementations(originalMethod, swizzledMethod);
        }
    });
}
```
