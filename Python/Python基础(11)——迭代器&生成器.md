##迭代器
迭代是访问集合元素的一种方式。迭代器是一个可以记住遍历的位置的对象。迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。
1. 可迭代对象
以直接作用于 for 循环的数据类型有以下几种：
一类是集合数据类型，如 list 、 tuple 、 dict 、 set 、 str 等；
一类是 generator ，包括生成器和带 yield 的generator function。
这些可以直接作用于 for 循环的对象统称为可迭代对象： Iterable 。
####isinstance
```
from collections import Iterable
str = "Hello Python"
print(isinstance(str,Iterable))
```
####dir
判断是不是含有__iter__
```
str = 'Hello Python' 
print('__iter__' in dir(str))
```
####iter()函数
可以被next()函数调用并不断返回下一个值的对象称为迭代器：Iterator。
生成器都是 Iterator 对象，但 list 、 dict 、 str 虽然是 Iterable ，却不是 Iterator 。把 list 、 dict 、 str 等 Iterable 变成 Iterator 可以使用 iter() 函数：
总结
凡是可作用于 for 循环的对象都是 Iterable 类型；
凡是可作用于 next() 函数的对象都是 Iterator 类型
集合数据类型如 list 、 dict 、 str 等是 Iterable 但不是 Iterator ，不过可以通过 iter() 函数获得一个 Iterator 对象。
## 生成器
```
for i in range(1, 10, 1):
    print(i)

def customRange(star, stop, step):
    x = star
    while x < stop:
        yield x
        x += step

for i in customRange(1, 10, 0.5):
    print(i)

```
