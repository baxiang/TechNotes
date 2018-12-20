python装饰器就是用于拓展原来函数功能的一种函数，这个函数的特殊之处在于它的返回值也是一个函数（函数的指针），使用python装饰器的好处就是在不用更改原函数的代码前提下给函数增加新的功能
给当前代码增加代码执行时间
```
import time

def func():
    beginTime = time.time()
    print("hello world")
    time.sleep(2)
    print("hello python")
    endTime = time.time()
    print(endTime-beginTime)

func()

```
如果需要在其他函数也增加代码执行时间，工作存在很大的重复性
```
import time

def foo():
    print("hello world")
    time.sleep(2)
    print("hello python")


def decorator(func):
    beginTime = time.time()
    func()
    endTime = time.time()
    print(endTime - beginTime)


if __name__=='__main__':
    f = foo
    decorator(f)  # 只有把func()或者f()作为参数执行，新加入功能才会生效
    print("
```
装饰器表示
```
import time

def deco(func):
    def wrapper():
        beginTime = time.time()
        func()
        endTime = time.time()
        print(endTime - beginTime)
    return wrapper

@deco
def foo():
    print("hello world")
    time.sleep(2)
    print("hello python")


if __name__=='__main__':
    foo()
```
带有参数的装饰器
```
import time


def addDeco(func):
    def wrapper(a,b):
        beginTime = time.time()
        func(a,b)
        endTime = time.time()
        print(endTime - beginTime)

    return wrapper


@addDeco
def bar(a,b):
    print("bar func")
    time.sleep(1)
    print("add result:",a+b)


if __name__=='__main__':
    bar(4,6)

```

