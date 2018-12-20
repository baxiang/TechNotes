　　运行时的文章一直被同学们热炒，当然现在面试中也都喜欢问道，当大伙说的头头是道时候，可到真正的项目中几乎局限只会关联对象或者MethodSwizzling奉为神剑到处挥砍，开发毕竟不能纸上谈兵，实践出真知，介绍目前在项目中runtime的具体使用，真切希望和各位同学探讨。
##1 关联对象（AssociatedObject ）
　　Catagory主要为已经存在的类(主要是系统类)扩展新的方法,关联对象是runtime在开发中应用的最广泛，其主要用于为Catagory的对象增加属性。
![AFNetworking的关联对象的](http://upload-images.jianshu.io/upload_images/143845-f3c0eeec69008bb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Masony的关联的对象](http://upload-images.jianshu.io/upload_images/143845-b2abae3821dfdf16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
关于分类的介绍可以查看美团技术团队写的[深入理解Objective-C:Category](http://tech.meituan.com/DiveIntoCategory.html)
####1.1 为什么catagory 无法设置属性

```
struct objc_category {
    char *category_name                                      OBJC2_UNAVAILABLE;
    char *class_name                                         OBJC2_UNAVAILABLE;
    struct objc_method_list *instance_methods                OBJC2_UNAVAILABLE;
    struct objc_method_list *class_methods                   OBJC2_UNAVAILABLE;
    struct objc_protocol_list *protocols                     OBJC2_UNAVAILABLE;
}                                                            OBJC2_UNAVAILABLE;
```
分类中可以添加实例方法，类方法，甚至可以实现协议，添加属性，不可以添加成员变量。主要因为方法定义都在objc_class中管理的，不管如何增删方法，都不影响类实例的内存布局，创建一个对象必然会分配一块内存区域，包含了isa指针和所有的成员变量。假如允许动态修改类成员变量布局，已经创建出的类实例就不符合类定义了，变成了无效对象。
####1.2 相关函数
```
//为一个实例对象添加一个关联对象，由于是C函数只能使用C字符串，这个key就是关联对象的名称，value为具体的关联对象的值，policy为关联对象策略，与我们自定义属性时设置的修饰符类似
void objc_setAssociatedObject(id object, const void *key, id value, objc_AssociationPolicy policy);
//通过key和实例对象获取关联对象的值
id objc_getAssociatedObject(id object, const void *key);
//删除实例对象的关联对象
void objc_removeAssociatedObjects(id object);
```
(1)key值
　　关于前两个函数中的 key 值是我们需要重点关注的一个点，这个 key 值必须保证是一个对象级别（为什么是对象级别？看完下面的章节你就会明白了）的唯一常量。一般来说，有以下三种推荐的 key 值：
声明 static char kAssociatedObjectKey; ，使用 &kAssociatedObjectKey 作为 key 值;
声明 static void *kAssociatedObjectKey = &kAssociatedObjectKey; ，使用 kAssociatedObjectKey 作为 key 值；
用 selector ，使用 getter 方法的名称作为 key 值。
static char kAssociatedObjectKey;
```
objc_getAssociatedObject(self, &kAssociatedObjectKey);
```
但是还有更简单的方法, 可以使用selector:
```
objc_getAssociatedObject(self, @selector(associatedObject));
```
或者直接使用_cmd: _cmd在Objective-C的方法中表示当前方法的selector, 正如同self表示调用当前方法的对象(类)一样.
```
objc_getAssociatedObject(self, _cmd);
```

(2) 关联规则 objc_AssociationPolicy policy和property使用的修饰符神似，具体含义也与property修饰符相同。
![image.png](http://upload-images.jianshu.io/upload_images/143845-10b588e204026b89.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

objc_AssociationPolicy | modifier 
----|------
OBJC_ASSOCIATION_ASSIGN	|assign
OBJC_ASSOCIATION_RETAIN_NONATOMIC |	nonatomic, strong
OBJC_ASSOCIATION_COPY_NONATOMIC	|nonatomic, copy
OBJC_ASSOCIATION_RETAIN	|atomic, strong
OBJC_ASSOCIATION_COPY	|atomic, copy

（3）objc_removeAssociatedObjects函数实际运用很少，它会移除一个对象的所有关联对象，将该对象恢复成“原始”状态。这样做就很有可能把别人添加的关联对象也一并移除，这并不是我们所希望的。所以一般的做法是通过给 objc_setAssociatedObject 函数传入 nil 来移除某个已有的关联对象。
####1.4 category关联对象的大体原理
 isa 结构体中的标记位 has_assoc 标记为 true，表示当前对象有关联对象，关联对象并不是成员变量，关联对象是由一个全局哈希表存储的键值对中的值。

##2 对象关系映射(ORM)
通过逆向APP会发现目前对象转模型这块目前主要用的是MJExtension和YYModel,老项目一般是MJExtension，新崛起的项目转到了YYModel上。利用runtime 我们可以实现json数据的直接转换成对象模型，或者把模型通过映射拼接成晦涩的sql语句，间接实现了对象存储到sqlite数据库 
![MJExtension](http://upload-images.jianshu.io/upload_images/143845-f83e2bc1e60279d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![YYModel中的YYClassInfo](http://upload-images.jianshu.io/upload_images/143845-9e6924ab83ac5bd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
其中ORM主要涉及到一下方法：
获取属性列表
```
objc_property_t *propertyList = class_copyPropertyList([self class], &count);
for (unsigned int i=0; i<count; i++) {
   const char *propertyName = property_getName(propertyList[i]);
   NSLog(@"property---->%@", [NSString stringWithUTF8String:propertyName]);
}
```
获取成员变量列表
```
Ivar *ivarList = class_copyIvarList([self class], &count);
for (unsigned int i; i<count; i++) {
    Ivar myIvar = ivarList[i];
    const char *ivarName = ivar_getName(myIvar);
    NSLog(@"Ivar---->%@", [NSString stringWithUTF8String:ivarName]);
}
```
获取方法列表
```
unsigned int count;
  Method *methodList = class_copyMethodList([self class], &count);
  for (int i = 0; i < count; i++) {
      Method method = methodList[i];
      DebugLog(@"------getRunTimeMethodList: %@",NSStringFromSelector(method_getName(method)));
  }
```
##3 热修复（HotfixPatch）
苹果审核一直被开发者吐槽的，一是苹果审核的严格，各种理由反反复复被打回去欲哭无泪，二是审核周期长，在2017年之前苹果审核的周期一般都在三天，如果是新应用甚至需要一周以上，如果碰上圣诞节苹果放假我们这边是一般都不会提交审核，于是JSPatch 为代表的热修复技术被开发者推崇，通过逆向中国市面上有头有脸的iOS应用，我发现几乎都使用JSPath或者JSPath的变种。以至于苹果发邮件禁止使用热修复时 整个JSPath的Issues被炸锅了。热修复主要做的是替换现有的方法，或者增加新方法，需要对消息发送和转发有一定的理解。

####3.1 消息转发_objc_msgForward
```
-[*** ***]:unrecognized selector sent to instance 0x*****
```
这个是ios开发中最常见的crash,当前对象找不到这个方法，实际上苹果 调用doesNotRecognizeSelector方法的时候，是给了我们三次机会的。就是我们常说的消息转发，
举一个栗子，我在工作中项目出现了差错，本着挽救同志的目的，领导让我立即马上提供一次挽回的方法，如果我给力这个危机到此没了，但是我跪了搞不定,领导就问谁可以解决，这是老王站了出来，如果老王接盘搞定了这个危机那也没事了，但是老王也没有解决 领导就会找小李啊或者小张处理，如果大家都沉默无法解决， 那就项目彻底破产啦。oc中消息转发差不多就是这样的。
```
+(BOOL)resolveInstanceMethod:(SEL)sel// 实例方法
+(BOOL)resolveClassMethod:(SEL)sel // 类方法
```
第一次机会允许用户在此时为该Class动态添加实现。如果有实现了，则调用并返回。
```
BOOL class_addMethod(Class cls, SEL name, IMP imp, const char *types);
```
如果仍没实现，继续下面的动作。
```
-(id)forwardingTargetForSelector:(SEL)aSelector 
```
调用forwardingTargetForSelector:方法，尝试找到一个能响应该消息的对象。如果获取到，则直接转发给它。如果返回了nil，继续下面的动作。
```
- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector
- (void)forwardInvocation:(NSInvocation *)anInvocation
```
通过 -methodSignatureForSelector: 消息获得函数的参数和返回值类型。如果 -methodSignatureForSelector: 返回 nil ，Runtime 则会发出 -doesNotRecognizeSelector: 消息，程序这时也就挂掉了。如果返回了一个函数签名，Runtime 就会创建一个 NSInvocation 对象并发送 -forwardInvocation: 消息给目标对象。
NSInvocation 是一个消息体的封装，包括selector 以及参数等信息。因此JSPatch通过NSInvocation来创建消息
![JSPatch](http://upload-images.jianshu.io/upload_images/143845-cffcec3c471bb143.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

NSInvocation可以实现传递多个参数。
```
- (void)viewDidLoad {
    [super viewDidLoad];
    //创建一个函数签名，这个签名可以是任意的,但需要注意，签名函数的参数数量要和调用的一致。
    NSMethodSignature * signature = [[self class] instanceMethodSignatureForSelector:@selector(testFun:argb:)];
    NSLog(@"参数个数%lu---返回参数类型%s",signature.numberOfArguments,signature.methodReturnType);
    //通过签名初始化
    NSInvocation * invocatin = [NSInvocation invocationWithMethodSignature:signature];
    NSString *argumentOne = @"First";
    NSString *argumentTwo = @"Two";
    //atIndex的下标必须从2开始。原因为：0 1 两个参数已经被target和selector占用
    [invocatin setArgument:&argumentOne atIndex:2];
    [invocatin setArgument:&argumentTwo atIndex:3];
    [invocatin setTarget:self];//设置target
    [invocatin setSelector:@selector(testFun:argb:)];//设置selecteor
    [invocatin invoke];//消息调用
}
-(NSString*)testFun:(NSString*)argc argb:(NSString*)argb{
    //实现 [argc stringByAppendingString:argb];
    NSString* string = argc;
    NSString* aString;
    NSString* stringToAppend = argb;
    NSInvocation* inv = [NSInvocation invocationWithMethodSignature:[NSString instanceMethodSignatureForSelector:@selector(stringByAppendingString:)]];
    [inv setTarget: string];
    [inv setSelector:@selector(stringByAppendingString:)];
    [inv setArgument:&stringToAppend atIndex:2];
    [inv retainArguments];
    [inv invoke];
    // 获取返回值
    [inv getReturnValue:&aString];
    return aString;
}
```
##4 私有变量的修改
主要是利用class_copyIvarList获取当前类的所有属性，主要为了获取私有变量然后利用KVC修改对象的属性。
通过打印UITextField的属性，获取到变量名称为_placeholderLabel，可以修改placeholder字体颜色。
```
unsigned int outCount = 0;
   Ivar *ivars  = class_copyIvarList([UITextField class], &outCount);
    for (NSInteger i = 0; i < outCount; ++i) {
        // 遍历取出该类成员变量
        Ivar ivar = *(ivars + i);
        NSLog(@"\n name = %s  \n type = %s", ivar_getName(ivar),ivar_getTypeEncoding(ivar));
    }
```
KVC 修改属性值
```
[_textView setValue: [UIColor redColor] forKeyPath:@"_placeholderLabel.textColor"];
```
一般上面写法用的很少，尽快替换了方法还是有很多坑等着
一般我们的用法直接KVC 替换系统原有的变量
```
UITextField *textFiled = [[UITextField alloc] initWithFrame:CGRectMake(20, 100, 100, 50)];
    [self.view addSubview:textFiled];
    UILabel *placeholderLabel = [UILabel new];
    placeholderLabel.textColor = [UIColor redColor];
    [placeholderLabel sizeToFit];
    placeholderLabel.text = @"请输入密码";
    [textFiled addSubview:placeholderLabel];
    [textFiled setValue:placeholderLabel forKey:@"_placeholderLabel"];
```
##5 面向切面编程（AOP）
主要利用Method Swizzling 在不破话原有的代码，将独立的功能模块剥离出来，实现代码注入。
####5.1 Method Swizzling
```
+ (BOOL)swizzleInstanceMethod:(SEL)originalSel with:(SEL)newSel {
    Method originalMethod = class_getInstanceMethod(self, originalSel);
    Method newMethod = class_getInstanceMethod(self, newSel);
    if (!originalMethod || !newMethod) return NO;
    
    class_addMethod(self,
                    originalSel,
                    class_getMethodImplementation(self, originalSel),
                    method_getTypeEncoding(originalMethod));
    class_addMethod(self,
                    newSel,
                    class_getMethodImplementation(self, newSel),
                    method_getTypeEncoding(newMethod));
    
    method_exchangeImplementations(class_getInstanceMethod(self, originalSel),
                                   class_getInstanceMethod(self, newSel));
    return YES;
}

+ (BOOL)swizzleClassMethod:(SEL)originalSel with:(SEL)newSel {
    Class class = object_getClass(self);
    Method originalMethod = class_getInstanceMethod(class, originalSel);
    Method newMethod = class_getInstanceMethod(class, newSel);
    if (!originalMethod || !newMethod) return NO;
    method_exchangeImplementations(originalMethod, newMethod);
    return YES;
}

```
 　　最重要的是需要理解selector, method, implementation 三者之间关系：在运行时，类（Class）维护了一个消息分发列表来解决消息的正确发送。每一个消息列表的入口是一个方法（Method），这个方法映射了一对键值对，其中键值是这个方法的名字 selector（SEL），值是指向这个方法实现的函数指针 implementation（IMP）。 Method swizzling 修改了类的消息分发列表使得已经存在的 selector 映射了另一个实现 implementation，同时重命名了原生方法的实现为一个新的 selector。
![NSPipster的Method Swizzling](http://upload-images.jianshu.io/upload_images/143845-671e478a1cfa6e12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Method Swizzling需要注意的是:
(1)应该总在+load中执行,+load会在类初始加载时调用，和+initialize比较+load能保证在类的初始化过程中被加载。
 (2) dispatch_once中执行：swizzling会改变全局状态，所以在运行时采取一些预防措施，使用dispatch_once就能够确保代码不管有多少线程都只被执行一次。这将成为method swizzling的最佳实践。


####5.2日志打印 快速熟悉项目。
　　程序猿是跳槽率偏高的职业，如果去新公司做新项目还好说，一旦需要接手老项目的维护，商业项目可不是我们平常写的Demo的代码量,那代码中的逻辑结构瞬间会让新入职的小伙伴们懵逼，通过通过拦截点击事件，可以快速的熟悉代码的逻辑。
![image.png](http://upload-images.jianshu.io/upload_images/143845-12f5adbab8f5e53b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####5.3处理通用逻辑
　　比如在一些界面我们需要用户登录才能查看，最笨的办法实在实在需要的ViewController 添加判断登录的逻辑。下面这张截图是从Github的找到的利用AOP处理用户登录的代码，当然这个用继承基础类去写也是不错的，暂且不要在意写法的好坏 最起码我们程序开发提供了新的思路。
![处理用户登录](http://upload-images.jianshu.io/upload_images/143845-4bd9c55070d23b2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####5.4Crash的防范
 OC中容器类在空值nil 和数组越界都会直接导致我们app 的crash 我们一种处理方式是利用Category增加新方法中判断值是否为空或者越界，对于新工程我们使用大家约定使用容器的category还好说，
```
- (id)objectOrNilAtIndex:(NSUInteger)index {
    return index < self.count ? self[index] : nil;
}
```
但是对于老项目 我们难道需要修改所有的容器类方法？我们可以使用切面来修改。
```
+(void)load{

    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{

        Class __NSPlaceholderArray = NSClassFromString(@"__NSPlaceholderArray");
        [NSArray swizzleInstance:__NSPlaceholderArray origMethod:@selector(initWithObjects:count:) withMethod:@selector(RBSafe_initWithObjects:count:)];
        
        Class __NSArray = NSClassFromString(@"NSArray");
        Class __NSArrayI = NSClassFromString(@"__NSArrayI");//数组有内容obj类型才是__NSArrayI
        Class __NSSingleObjectArrayI = NSClassFromString(@"__NSSingleObjectArrayI");//iOS10 以上，单个内容类型是__NSArraySingleObjectI
        Class __NSArray0 = NSClassFromString(@"__NSArray0");//iOS9 以上，没内容类型是__NSArray0
        
        [NSArray swizzleInstance:__NSArray origMethod:@selector(subarrayWithRange:) withMethod:@selector(RBSafe_subarrayWithRange:)];
        
        [NSArray swizzleInstance:__NSArray origMethod:@selector(objectsAtIndexes:) withMethod:@selector(RBSafe_objectsAtIndexes:)];
   
        [NSArray swizzleInstance:__NSArrayI origMethod:@selector(objectAtIndex:) withMethod:@selector(RBSafe_NSArrayIobjectAtIndex:)];
        
        [NSArray swizzleInstance:__NSSingleObjectArrayI origMethod:@selector(objectAtIndex:) withMethod:@selector(RBSafe_NSSingleObjectArrayIobjectAtIndex:)];
        
        [NSArray swizzleInstance:__NSArray0 origMethod:@selector(objectAtIndex:) withMethod:@selector(RBSafe_NSArray0ObjectAtIndex:)];
        
    });
}

```
　　当然这种用法 我个人是持中立态度的，因为可以瞬间把我们代码所犯的错误处理的风平浪静，但是让我有一种掩耳盗铃的感觉，我们的问题和错误根源还在的，不断的错误叠加只会让我们代码变得危机重重，同时AOP的crash处理是无痛无感知的，一旦我们运用在第三方的静态库实际上我们就会侵入被人工程的代码，被人的代码被篡改都不知情的，这个需要谨慎使用。

##6 逆向开发
　　逆向开发主要集中在iOS越狱方面，逆向开发可以让我们在iOS开发中打开另一扇门，对于大部门开发者来说很少接触这个领域，我也是在工作中才接触到iOS的越狱，逆向开发的基础就是利用Method Swizzling，不管是现在热门的THEOS还是iOSOpenDev都是Method Swizzling的封装，点击iOSOpenDev使用的CaptainHook就可以看到都是Method Swizzling 各种方法。
```
#import <Cycript/Cycript.h>
#import <CaptainHook/CaptainHook.h>

#define CYCRIPT_PORT 8888

CHDeclareClass(AppDelegate);
CHDeclareClass(UIApplication);

CHOptimizedMethod2(self, void, AppDelegate, application, UIApplication *, application, didFinishLaunchingWithOptions, NSDictionary *, options)
{
    CHSuper2(AppDelegate, application, application, didFinishLaunchingWithOptions, options);
    
    NSLog(@"## Start Cycript ##");
    CYListenServer(CYCRIPT_PORT);
}

__attribute__((constructor)) static void entry() {
    CHLoadLateClass(AppDelegate);
    CHHook2(AppDelegate, application, didFinishLaunchingWithOptions);
}
```
