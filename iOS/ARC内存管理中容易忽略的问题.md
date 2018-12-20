>目录：
一、字符串（String）
　1.1、字符串的创建
　1.2、字符串的isa
二、拷贝（copy）
　2.1、immutable对象的copy
　2.2、mutable对象的copy
　2.3、浅拷贝与深拷贝
    2.4 、单层深拷贝
三、 集合（Collections）
　3.1、NSMapTable
　3.2、NSHashTable
　3.3、NSPointerArray

##一、字符串（String）
　　看到好几篇文章都在说这道面试题，字符串差不多是每个高级语言必有的，在实际项目中也的确是使用的最多类型之一。本文就以此题开始我们的内存管理的讨论。
```
  //第一个
   NSString *name1 = [NSString stringWithFormat:@"stringTestOne"];
    __weak NSString *name2 = name1;
    NSLog(@"name1:%@", name1);
    NSLog(@"name2:%@", name2);
    name1 = nil;
    NSLog(@"name1:%@", name1);
    NSLog(@"name2:%@", name2);
    //第二个
    NSString *a1 = [[NSString alloc] initWithFormat:@"stringTestTwo"];
    __weak NSString *a2 = a1;
    NSLog(@"a1:%@", a1);
    NSLog(@"a2:%@", a2);
    a1 = nil;
    NSLog(@"a1:%@", a1);
    NSLog(@"a2:%@", a2);
     //第三个
    NSString *b1 = @"stringTestThree";
    __weak NSString *b2 = b1;
    NSLog(@"b1:%@", b1);
    NSLog(@"b2:%@", b2);
    b1 = nil;
    NSLog(@"b1:%@", b1);
    NSLog(@"b2:%@", b2);

打印结果：
  name1:stringTestOne
  name2:stringTestOne
  name1:(null)
  name2:stringTestOne
  a1:stringTestTwo
  a2:stringTestTwo
  a1:(null)
  a2:(null)
  b1:stringTestThree
  b2:stringTestThree
  b1:(null)
  b2:stringTestThree
```
　　暂且不去喷面试官问这个到底有大的装ｘ成分在里面，但是我真觉得这道题考察的意义不大，而且存在漏洞的。
![狗货](http://upload-images.jianshu.io/upload_images/143845-877d6a4d88dc4526.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####1.1、字符串的创建
```
   NSString *string1 = [NSString stringWithFormat:@"TestString1_BAXIANG"];
    NSLog(@"%p",string1);
    NSString *string2 = [[NSString alloc] initWithFormat:@"TestString2_BAXIANG"];
    NSLog(@"%p",string2);
    NSString *string3 = @"TestString3_BAXIANG";
    NSLog(@"%p",string3);
打印结果：
 0x7fb6e7d58a40
 0x7fb6e7d1c3a0
 0x10e6a7280
```
　　(1) 关于stringWithFormat和initWithFormat的区别如果同学是从MRC开发者一路过来的话理解这个很简单，但是ARC已经彻底主导了如今的开发，对引用计数这个概念不需要理解那么苛刻，stringWithFormat实际上创建的是一个加入自动释放池的 （autoreleased)的对象，主要目的就是延迟释放，而initWithFormat的对象就需要遵循我们常唠叨的内存管理黄金法则 谁创建谁释放。也就是MRC中的release。

　　通过po _objc_autoreleasePoolPrint()打印当前自动释放池的中对象(Autorelease pools)，刚才我们通过stringWithFormat创建的字符串对象0x7fa65a50fdc0 在当前释放池中。
```
objc[24294]: ##############
objc[24294]: AUTORELEASE POOLS for thread 0x10e6533c0
objc[24294]: 798 releases pending.
objc[24294]: [0x7fa65b800000]  ................  PAGE (full)  (cold)
objc[24294]: [0x7fa65b800038]  ################  POOL 0x7fa65b800038
objc[24294]: [0x7fa65b800040]    0x7fa65a702820  __NSCFString
objc[24294]: [0x7fa65b800048]  ################  POOL 0x7fa65b800048
........
// 
objc[24294]: [0x7fa65b010958]    0x7fa65a50fdc0  __NSCFString
```
　　所以第一个字符name1 = nil 只是去掉了对当前0x7fa65a50fdc0 字符的指针。name2还是指向了0x7fa65a50fdc0字符串，所以name2还可以打印出当前的字符数据。而关于通过打印内存地址会发现字符串3(0x10e6a7280)会明显小于上面二者，因为它是创建在字符串常量区的，而我们的第一二字符串是创建在堆区的。所以b2是照样可以打印出字符串的。但是如果我们修改第二个字符串内容为string，在64位的苹果设备上调试，打印的结果变成跟我们的第一个和第二个字符串结果一样的：
```
  NSString *a1 = [[NSString alloc] initWithFormat:@"string"];
    __weak NSString *a2 = a1;
    NSLog(@"a1:%@", a1);
    NSLog(@"a2:%@", a2);
    a1 = nil;
    NSLog(@"a1:%@", a1);
    NSLog(@"a2:%@", a2);
打印结果：
a1:string
a2:string
a1:(null)
a2:string
```
　　可能看到这个结果会觉得颠覆我们上面的理论 ，但是我们看到下面截图也许你就会更加奇怪：

![字符内容是:stringTestTwo](http://upload-images.jianshu.io/upload_images/143845-ecbf28371592578a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![字符内容是:string](http://upload-images.jianshu.io/upload_images/143845-f3b6612ed88f7eab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

　　我们只是缩短了字符串的长度，当前的字符串的类就变了 ，更让人奇怪的是字符缩短后的对象没有isa是空。也就是当前字符串对象没有类。这就涉及到apple的**Tagged Pointer**。

####1.2、字符串的isa
（1）NSTaggedPointerString
　　NSTaggedPointerString 用指针地址的富余位存储当前变量值，若对象指针的最低有效位为1(即奇数)，则该指针为Tagged Pointer。这种指针不通过解引用isa来获取其所属类，而是通过接下来三位的一个类表的索引。该索引是用来查找所属类是采用Tagged Pointer的哪个类。剩下的60位则存储数据。对于NSNumber，小于2^60-1的整数就都采用Tagged Pointer来存储，对于字符串来说所需内存小于60位的，它可以创建一个Tagged Pointer，所以NSTaggedPointerString是一个伪装的对象，里面存储的不是指针地址而是字符串值，这样不需要一次真正对象的内存分配，不需要一次间接取值。同时引用计数可以是空指令，因为没有内存需要释放，所以会对性能有显著的提升。

（2）__NSCFConstantString

　　字符串常量，是一种编译时常量，它的 retainCount 值很大，在控制台打印出的数值则是 18446744073709551615==2^64-1，测试证明，即便对其进行 release 操作，retainCount 也不会产生任何变化。相同内容的 __NSCFConstantString 对象的地址相同，也就是说常量字符串对象是一种单例。这种对象一般通过字面值 @"..."、CFSTR("...") 或者 stringWithString: 方法（需要说明的是，这个方法在 iOS6 SDK 中已经被称为redundant，使用这个方法会产生一条编译器警告。这个方法等同于字面值创建的方法）产生。这种对象存储在字符串常量区。
（3）__NSCFString
　　 对象被存储在堆上。 __NSCFString 对象是在运行时创建的一种 NSString 子类，他并不是一种字符串常量。所以和其他的对象一样在被创建时获得了 1 的引用计数。通过 NSString 的 stringWithFormat 等方法创建的 NSString 对象一般都是这种类型。

##二、拷贝（copy）

####2.1、immutable对象的copy
　　向immutable对象发送copy消息一定会得到一个新对象吗？下面的测试demo中向不可变的NSString、NSArray、NSDictionary以及NSSet对象发送copy消息，得到了immutable的新对象，但是问题是：copy是深拷贝还是浅拷贝了？
```
    NSString *testStr = @"abc";
    NSString *copyStr = [testStr copy];
    NSLog(@"testStr = %p", testStr);
    NSLog(@"copyStr = %p", copyStr);
   
    NSArray *testArray = @[@1, @2, @3];
    NSArray *copyArray = [testArray copy];
    NSLog(@"testArray  = %p", testArray);
    NSLog(@"copyArray  = %p", copyArray);
  
    NSSet *testSet = [NSSet setWithObjects:@1,@2,@3,nil];
    NSSet *copySet = [testSet copy];
    NSLog(@"testSet  = %p", testSet);
    NSLog(@"copySet  = %p", copySet);
    
    NSDictionary *testDict = @{@"testKey":@"testValue"};
    NSDictionary *copyDict= [testDict copy];
    NSLog(@"testDict = %p", testDict);
    NSLog(@"testDict = %p", copyDict);
```
打印结果：
```
testStr = 0x10442f220
copyStr = 0x10442f220
testArray  = 0x7f9e7b60c3a0
copyArray  = 0x7f9e7b60c3a0
testSet  = 0x7f9e7b515e00
copySet  = 0x7f9e7b515e00
testDict = 0x7f9e7b799140
testDict = 0x7f9e7b799140
```
向immutable对象发送copy消息，这些对象会直接返回本身，而不是返回一个新创建的对象。
```
- (id)copy {
    return [(id)self copyWithZone:nil];
}
- (id)mutableCopy {
    return [(id)self mutableCopyWithZone:nil];
}
```
``` --obj
- (id)copyWithZone:(NSZone *)zone{
 if (NSStringClass == Nil) 
    NSStringClass = [NSString class]; 
 return RETAIN(self); 
// return [[NSStringClass allocWithZone:zone] initWithString:self];
}

/* NSMutableCopying methods */
- (id)mutableCopyWithZone:(NSZone*)zone{ 
      return [[NSMutableString allocWithZone:zone] initWithString:self];
}
```
####2.2、mutable对象的copy
```
  NSMutableArray *array = [NSMutableArray arrayWithObjects:@"baxianga",@"baxiang1",@"12345",nil];
    NSArray *copyArray = [array copy];
    NSMutableArray *mCopyArray = [array mutableCopy];
    NSLog(@"array = %p", array);
    NSLog(@"copyArray = %p", copyArray);
    NSLog(@"mCopyArray = %p", mCopyArray);
```
copyArray、mCopyArray和array的内存地址都不一样，说明copyArray、mCopyArray都对array进行了内容拷贝。
####2.3、浅拷贝与深拷贝
　　对象拷贝有两种方式：浅拷贝（指针复制）和深拷贝（内容复制），浅拷贝，并不拷贝对象内容，仅仅是拷贝指向对象的指针；深拷贝是直接拷贝整个对象内容到另一块内存中。对immutable对象进行copy，是浅拷贝，mutableCopy是深拷贝；对mutable对象进行copy和mutableCopy都是深拷贝。表示如下：
```
[immutableObject copy] // 浅拷贝
[immutableObject mutableCopy] //深拷贝
[mutableObject copy] //深拷贝
[mutableObject mutableCopy] //深拷贝
```
#### 2.4 单层深拷贝
　　集合对象的深拷贝仅限于对象本身，对象元素仍然是浅拷贝。苹果称为单层深拷贝（This kind of copy is only capable of producing a one-level-deep copy），一个真正的深拷贝是[Deep Copies](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Collections/Articles/Copying.html#//apple_ref/doc/uid/TP40010162-SW3)
```
NSArray* trueDeepCopyArray = [NSKeyedUnarchiver unarchiveObjectWithData:
[NSKeyedArchiver archivedDataWithRootObject:oldArray]];
```

##三、 集合（Collections）
　　集合存储一个对象，只是对这个对象的引用，也就是我们说的单层深拷贝事情，使该对象的retainCount+1，当从数组中移除对象时，则引用计数retainCount-1。
　　ARC下retainCount是无法使用的，获取引用计数(retain count)]的三种方法，虽然不是很准确，但是还是可以鉴别一下当前内存的
（1) 私有方法
```
 OBJC_EXTERN int _objc_rootRetainCount(id);
 NSLog(@"before----%d",_objc_rootRetainCount(fo));
```
（2) 使用KVC
```
 NSLog(@"%@",[fo valueForKey:@"retainCount"]);
```
 (3) Core Foundation
```
 NSLog(@"%ld",CFGetRetainCount((__bridge CFTypeRef)(fo)));
```
iOS集合类中添加一个对象，会使对象的引用计数加1，被数组所持有。但是有时候不希望集合对象对存储的对象进行引用计数，这个时候就可以用到NSMapTable／NSHashTable／NSPointerArray。
```
    Foo *fo = [[Foo alloc] init];
    NSLog(@"before----%d",_objc_rootRetainCount(fo));
    NSMutableArray *array = [NSMutableArray array];
    [array addObject:fo];
    NSLog(@"after----%d",_objc_rootRetainCount(fo));
  // 打印结果：
   before----1
   after----2
```
####3.1、NSMapTable
　　NSMapTable类似NSDictionary ,NSDcitionary或者NSMutableDictionary中对于key和value的内存管理是，对key进行copy，对value进行强引用。
```
+ (instancetype)dictionaryWithObject:(ObjectType)object forKey:(KeyType <NSCopying>)key;
```
　　为了保证这个特性在NSDcitionary中对key的内存管理为copy，在复制的时候需要考虑对系统的负担，因此key应该是轻量级的，所以通常我们都用字符串和数字来做索引，但这只能说是key-to-object映射，不能说是object-to-object的映射。
```
- (instancetype)initWithKeyOptions:(NSPointerFunctionsOptions)keyOptions
                      valueOptions:(NSPointerFunctionsOptions)valueOptions
                          capacity:(NSUInteger)initialCapacity
```
```
static const NSPointerFunctionsOptions NSMapTableStrongMemory ;
static const NSPointerFunctionsOptions NSMapTableZeroingWeakMemory;
static const NSPointerFunctionsOptions NSMapTableCopyIn;
static const NSPointerFunctionsOptions NSMapTableObjectPointerPersonality;
static const NSPointerFunctionsOptions NSMapTableWeakMemory;
```
```
Person *p1 = [[Person alloc] initWithName:@"jack"];
Favourite *f1 = [[Favourite alloc] initWithName:@"ObjC"];

Person *p2 = [[Person alloc] initWithName:@"rose"];
Favourite *f2 = [[Favourite alloc] initWithName:@"Swift"];

NSMapTable *MapTable = [NSMapTable mapTableWithKeyOptions:NSMapTableWeakMemory valueOptions:NSMapTableWeakMemory];
// 设置对应关系表
// p1 => f1;
// p2 => f2
[MapTable setObject:f1 forKey:p1];
[MapTable setObject:f2 forKey:p2];

NSLog(@"%@ %@", p1, [MapTable objectForKey:p1]);
NSLog(@"%@ %@", p2, [MapTable objectForKey:p2]);
```

####3.2、NSHashTable
　　NSHashTable类似于NSSet和NSMutableSet合体，NSHashTable是可变的,可以使用
NSHashTableWeakMemory ，此选项使用weak存储对象，当对象被销毁的时候自动将其从集合中移除。NSHashTableObjectPointerPersonality ：和 NSPointerFunctionsObjectPointerPersonality 相同，此选项是直接使用指针进行isEqual: 和 hash，提高查找效率，为了防止循环引用，创建一个delegate管理器，运营的就是NSHashTable。
```
- (NSHashTable *)delegates
{
    if (!_delegates) {
        _delegates = [NSHashTable weakObjectsHashTable];
    }
    
    return _delegates;
}

- (void)addDelegate:(id<UserAuthNotifierDelegate>)delegate
{
    if ([self.delegates containsObject:delegate]) {
        return;
    }
    [self.delegates addObject:delegate];
}
- (void)removeDelegate:(id<UserAuthNotifierDelegate>)delegate
{
    [self.delegates removeObject:delegate];
}
```
####3.3、NSPointerArray
　　类似与NSArray ,NSPointerArray可以默认成 mutable的，而且可以插入空值nil,我们可以设置存储对象是否引用
```
[NSPointerArray strongObjectsPointerArray];// 强引用
[NSPointerArray weakObjectsPointerArray]; // 弱引用
```
