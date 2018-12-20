```
struct objc_class {
    struct objc_class *isa;
};
struct objc_object {
    struct objc_class *isa;
};
/// An opaque type that represents an Objective-C class.
typedef struct objc_class *Class; //类  (class object)
/// A pointer to an instance of a class.
typedef struct objc_object *id;   //对象 (instance of class)
```
objc_class结构体内，有一个Class类型的变量叫isa，由上面可以知道Class是一个objc_class指针，因此isa是一个objc_class指针，通常如果在一个objc_object（下面会说到）中，也会有一个isa指针，指向的是这个对象所对应的类（objc_class）。 如果是在objc_class中的isa指针，指向的则是这个类的元类(metaClass）

创建类

![image.png](http://upload-images.jianshu.io/upload_images/143845-e88cee03882fdfea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/143845-fc316db2c200a1dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
struct objc_object {
private:
    isa_t isa;

public:

    // ISA() assumes this is NOT a tagged pointer object
    Class ISA();

    // getIsa() allows this to be a tagged pointer object
    Class getIsa();

    // initIsa() should be used to init the isa of new objects only.
    // If this object already has an isa, use changeIsa() for correctness.
    // initInstanceIsa(): objects with no custom RR/AWZ
    // initClassIsa(): class objects
    // initProtocolIsa(): protocol objects
    // initIsa(): other objects
    void initIsa(Class cls /*nonpointer=false*/);
    void initClassIsa(Class cls /*nonpointer=maybe*/);
    void initProtocolIsa(Class cls /*nonpointer=maybe*/);
    void initInstanceIsa(Class cls, bool hasCxxDtor);

    // changeIsa() should be used to change the isa of existing objects.
    // If this is a new object, use initIsa() for performance.
    Class changeIsa(Class newCls);

    bool hasNonpointerIsa();
    bool isTaggedPointer();
    bool isBasicTaggedPointer();
    bool isExtTaggedPointer();
    bool isClass();

    // object may have associated objects?
    bool hasAssociatedObjects();
    void setHasAssociatedObjects();

    // object may be weakly referenced?
    bool isWeaklyReferenced();
    void setWeaklyReferenced_nolock();

    // object may have -.cxx_destruct implementation?
    bool hasCxxDtor();

    // Optimized calls to retain/release methods
    id retain();
    void release();
    id autorelease();

    // Implementations of retain/release methods
    id rootRetain();
    bool rootRelease();
    id rootAutorelease();
    bool rootTryRetain();
    bool rootReleaseShouldDealloc();
    uintptr_t rootRetainCount();

    // Implementation of dealloc methods
    bool rootIsDeallocating();
    void clearDeallocating();
    void rootDealloc();

private:
    void initIsa(Class newCls, bool nonpointer, bool hasCxxDtor);

    // Slow paths for inline control
    id rootAutorelease2();
    bool overrelease_error();

#if SUPPORT_NONPOINTER_ISA
    // Unified retain count manipulation for nonpointer isa
    id rootRetain(bool tryRetain, bool handleOverflow);
    bool rootRelease(bool performDealloc, bool handleUnderflow);
    id rootRetain_overflow(bool tryRetain);
    bool rootRelease_underflow(bool performDealloc);

    void clearDeallocating_slow();

    // Side table retain count overflow for nonpointer isa
    void sidetable_lock();
    void sidetable_unlock();

    void sidetable_moveExtraRC_nolock(size_t extra_rc, bool isDeallocating, bool weaklyReferenced);
    bool sidetable_addExtraRC_nolock(size_t delta_rc);
    size_t sidetable_subExtraRC_nolock(size_t delta_rc);
    size_t sidetable_getExtraRC_nolock();
#endif

    // Side-table-only retain count
    bool sidetable_isDeallocating();
    void sidetable_clearDeallocating();

    bool sidetable_isWeaklyReferenced();
    void sidetable_setWeaklyReferenced_nolock();

    id sidetable_retain();
    id sidetable_retain_slow(SideTable& table);

    uintptr_t sidetable_release(bool performDealloc = true);
    uintptr_t sidetable_release_slow(SideTable& table, bool performDealloc = true);

    bool sidetable_tryRetain();

    uintptr_t sidetable_retainCount();
#if DEBUG
    bool sidetable_present();
#endif
};
```
##objc_class
```
struct objc_class {
    Class isa  OBJC_ISA_AVAILABILITY;

#if !__OBJC2__
    Class super_class                                        OBJC2_UNAVAILABLE;
    const char *name                                         OBJC2_UNAVAILABLE;
    long version                                             OBJC2_UNAVAILABLE;
    long info                                                OBJC2_UNAVAILABLE;
    long instance_size                                       OBJC2_UNAVAILABLE;
    struct objc_ivar_list *ivars                             OBJC2_UNAVAILABLE;
    struct objc_method_list **methodLists                    OBJC2_UNAVAILABLE;
    struct objc_cache *cache                                 OBJC2_UNAVAILABLE;
    struct objc_protocol_list *protocols                     OBJC2_UNAVAILABLE;
#endif

} OBJC2_UNAVAILABLE;
```
object_getClass源码：
```
Class object_getClass(id obj)
{
    if (obj) return obj->getIsa();
    else return Nil;
}
```

通过clang Dog.m
```
clang -rewrite-objc Dog.m
```
```
#import <Foundation/Foundation.h>

@interface Animal : NSObject
@end

@implementation Animal
@end

@interface Dog : Animal {
    int age;
    NSString *name;
}
- (void)instanceFun;
+ (void)classFun;
@end

@implementation Dog
- (void)instanceFun {
}
+ (void)classFun {
}
@end

```

```
extern "C" __declspec(dllexport) struct _class_t OBJC_METACLASS_$_Animal;
extern "C" __declspec(dllimport) struct _class_t OBJC_METACLASS_$_NSObject;

extern "C" __declspec(dllexport) struct _class_t OBJC_METACLASS_$_Dog __attribute__ ((used, section ("__DATA,__objc_data"))) = {
	0, // &OBJC_METACLASS_$_NSObject,
	0, // &OBJC_METACLASS_$_Animal,
	0, // (void *)&_objc_empty_cache,
	0, // unused, was (void *)&_objc_empty_vtable,
	&_OBJC_METACLASS_RO_$_Dog,
};

extern "C" __declspec(dllexport) struct _class_t OBJC_CLASS_$_Animal;

extern "C" __declspec(dllexport) struct _class_t OBJC_CLASS_$_Dog __attribute__ ((used, section ("__DATA,__objc_data"))) = {
	0, // &OBJC_METACLASS_$_Dog,
	0, // &OBJC_CLASS_$_Animal,
	0, // (void *)&_objc_empty_cache,
	0, // unused, was (void *)&_objc_empty_vtable,
	&_OBJC_CLASS_RO_$_Dog,
};
static void OBJC_CLASS_SETUP_$_Dog(void ) {
	OBJC_METACLASS_$_Dog.isa = &OBJC_METACLASS_$_NSObject;
	OBJC_METACLASS_$_Dog.superclass = &OBJC_METACLASS_$_Animal;
	OBJC_METACLASS_$_Dog.cache = &_objc_empty_cache;
	OBJC_CLASS_$_Dog.isa = &OBJC_METACLASS_$_Dog;
	OBJC_CLASS_$_Dog.superclass = &OBJC_CLASS_$_Animal;
	OBJC_CLASS_$_Dog.cache = &_objc_empty_cache;
}

```
```
static struct _class_ro_t _OBJC_METACLASS_RO_$_Dog __attribute__ ((used, section ("__DATA,__objc_const"))) = {
	1, sizeof(struct _class_t), sizeof(struct _class_t), 
	(unsigned int)0, 
	0, 
	"Dog",
	(const struct _method_list_t *)&_OBJC_$_CLASS_METHODS_Dog,
	0, 
	0, 
	0, 
	0, 
};

static struct _class_ro_t _OBJC_CLASS_RO_$_Dog __attribute__ ((used, section ("__DATA,__objc_const"))) = {
	0, __OFFSETOFIVAR__(struct Dog, age), sizeof(struct Dog_IMPL), 
	(unsigned int)0, 
	0, 
	"Dog",
	(const struct _method_list_t *)&_OBJC_$_INSTANCE_METHODS_Dog,
	0, 
	(const struct _ivar_list_t *)&_OBJC_$_INSTANCE_VARIABLES_Dog,
	0, 
	0, 
};

extern "C" __declspec(dllexport) struct _class_t OBJC_METACLASS_$_Animal;
extern "C" __declspec(dllimport) struct _class_t OBJC_METACLASS_$_NSObject;

extern "C" __declspec(dllexport) struct _class_t OBJC_METACLASS_$_Dog __attribute__ ((used, section ("__DATA,__objc_data"))) = {
	0, // &OBJC_METACLASS_$_NSObject,
	0, // &OBJC_METACLASS_$_Animal,
	0, // (void *)&_objc_empty_cache,
	0, // unused, was (void *)&_objc_empty_vtable,
	&_OBJC_METACLASS_RO_$_Dog,
};
```
实例方法和实例变量应该被加到类中。类方法被加到元类中。
```
static struct /*_method_list_t*/ {
	unsigned int entsize;  // sizeof(struct _objc_method)
	unsigned int method_count;
	struct _objc_method method_list[1];
} _OBJC_$_INSTANCE_METHODS_Dog __attribute__ ((used, section ("__DATA,__objc_const"))) = {
	sizeof(_objc_method),
	1,
	{{(struct objc_selector *)"instanceFun", "v16@0:8", (void *)_I_Dog_instanceFun}}
};

static struct /*_method_list_t*/ {
	unsigned int entsize;  // sizeof(struct _objc_method)
	unsigned int method_count;
	struct _objc_method method_list[1];
} _OBJC_$_CLASS_METHODS_Dog __attribute__ ((used, section ("__DATA,__objc_const"))) = {
	sizeof(_objc_method),
	1,
	{{(struct objc_selector *)"classFun", "v16@0:8", (void *)_C_Dog_classFun}}
};
```
```
+ (Class)class {
    return self;
}

- (Class)class {
    return object_getClass(self);
}

```
```
BOOL class_isMetaClass(Class cls)
{
    if (!cls) return NO;
    return cls->isMetaClass();
}
 bool isMetaClass() {
        return info & CLS_META;
 }
```
