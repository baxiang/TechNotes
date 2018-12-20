##指向常量的指针
如果想让一个指针指向一个常量，声明的方式是在一个指针变量前面加上const,也是就被指向的对象是常量，所以*p 为常量，不能修改*p的值。
```
const int *p;
```

## 常量指针
const限定符在*号的右边 指针本身是一个const指针，因为这个指针本身就是一个常量，所以编译器要求给它一个初始化值，需要在申明的同时必须初始化指针。也就是指针p  为常量，初始化后不能再指向其它地址。
```
#include <stdio.h>
int main() {
    int x= 45;
    int const sum = 100;
    int *const p = &x;
    int *const p2 = ∑
    printf("%d \n%d\n",*p,*p2);
    int y = 55;
    x = y;
    printf("%d\n",*p);
    *p = sum;
    printf("%d\n",*p);
    int  *p1 = p;
    printf("%d",*p1);
    return 0;
}
```
##指向常量的常量指针
指向常量的指针可以先声明，后进行初始化，所以可以把指针指向非常量
##指向常量的指针指向普通变量
虽然*p无法负值，但可以直接修改变量的值来达到修改*p的效果
```
#include <stdio.h>
int main() {

    int x = 256;
    const  int y =88;
    const  int *p;
    int *p1;
    p =&y;
    printf("%d\n",*p);
    p = &x;
    printf("%d\n",*p);
    x =128;
    printf("%d\n",*p);
    p1 =(int *)&y;
    printf("%d\n",*p1);
    return 0;
}
```
