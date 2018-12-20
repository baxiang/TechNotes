##函数指针概念
```
#include <stdio.h>

int test(int x)
{
    return x*x;
}

typedef int (*func)(int x);
int main(int argc, char *argv[])
{
    func pt = test;
    printf("%d\n",pt(3));
    return 0;
}
```
## 函数指针做函数参数
