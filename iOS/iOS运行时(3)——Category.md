```
typedef struct objc_category *Category;
```

```
struct objc_category {
    char *category_name                           OBJC2_UNAVAILABLE; // 类别名称
    char *class_name                              OBJC2_UNAVAILABLE; // 类名
    struct objc_method_list *instance_methods     OBJC2_UNAVAILABLE; // 实例方法列表
    struct objc_method_list *class_methods        OBJC2_UNAVAILABLE; // 类方法列表
    struct objc_protocol_list *protocols          OBJC2_UNAVAILABLE; // 协议列表
}
```
