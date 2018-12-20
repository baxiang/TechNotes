####函数定义
定义函数使用def 关键字开头，后面是函数名称和圆括号()和冒号：
```
def 函数名 (参数列表)：
   函数体
def <funName>(arg1,arg2...agrN):
<statements>
```
```
def printStr(str):
    print(str)

printStr("hello world")
```
定义一个什么都不做的空函数，可以使用pass语句
```
def doNothing():
    pass

doNothing()
```
####关键字参数
关键字参数和函数的调用关系密切，函数调用的时候必须使用函数的参数名称确定传入的参数值一一对应
```
def userInfo(name, age):
    print("我的名字是%s,我今年%d岁" % (name, age))
userInfo("bx",25)
userInfo(name="user",age=23)
userInfo(age=28,name="ssss")
```
####默认参数
给参数设置一个默认值，如果调用函数的时候没有给参数赋值，就会直接使用设置的函数默认值
```
def personInfo(name,age="18"):
    print("姓名："+name,"年龄："+age)
personInfo("bx")
# 姓名：bx 年龄：18
```
#### 可变参数*args 
接收不定长的参数,参会类型是元组
```
def personInfo(*args):
    print(args)

personInfo("bx",25,"男")
# ('bx', 25, '男')
# {'name': 'bx', 'age': 25, 'gender': '男'}
```
####**kwargs
 接收键值对数据类型的参数
```
def personInfo(**kwargs):
    print(kwargs)

personInfo(name="bx",age=25,gender="男")
```
####闭包
1. 函数引用
```
def test1():
    print("--- in test1 func----")
```
####调用函数
```
test1()
```
####引用函数
```
ret = test1

print(id(ret))
print(id(test1))
```
通过引用调用函数
ret()
运行结果:
```
--- in test1 func----
140212571149040
140212571149040
--- in test1 func----
```
2. 什么是闭包
```
#定义一个函数
def test(number):
```
在函数内部再定义一个函数，并且这个函数用到了外边函数的变量，那么将这个函数以及用到的一些变量称之为闭包

```
    def test_in(number_in):
        print("in test_in 函数, number_in is %d"%number_in)
        return number+number_in
    #其实这里返回的就是闭包的结果
    return test_in

#给test函数赋值，这个20就是给参数number
ret = test(20)
#注意这里的100其实给参数number_in
print(ret(100))

#注意这里的200其实给参数number_in
print(ret(200))
```
运行结果：

```
in test_in 函数, number_in is 100
120

in test_in 函数, number_in is 200
220
```
3. 闭包再理解
内部函数对外部函数作用域里变量的引用（非全局变量），则称内部函数为闭包。
```
# closure.py

def counter(start=0):
    count=[start]
    def incr():
        count[0] += 1
        return count[0]
    return incr
启动python解释器

>>>import closeure
>>>c1=closeure.counter(5)
>>>print(c1())
6
>>>print(c1())
7
>>>c2=closeure.counter(100)
>>>print(c2())
101
>>>print(c2())
102
nonlocal访问外部函数的局部变量(python3)
def counter(start=0):
    def incr():
        nonlocal start
        start += 1
        return start
    return incr

c1 = counter(5)
print(c1())
print(c1())

c2 = counter(50)
print(c2())
print(c2())

print(c1())
print(c1())

print(c2())
print(c2())
```
4. 看一个闭包的实际例子：
```
def line_conf(a, b):
    def line(x):
        return a*x + b
    return line

line1 = line_conf(1, 1)
line2 = line_conf(4, 5)
print(line1(5))
print(line2(5))
```
这个例子中，函数line与变量a,b构成闭包。在创建闭包的时候，我们通过line_conf的参数a,b说明了这两个变量的取值，这样，我们就确定了函数的最终形式(y = x + 1和y = 4x + 5)。我们只需要变换参数a,b，就可以获得不同的直线表达函数。
####lambda 表达式
普通函数表达式
```
def add(x,y):
    return x+y
print(add(1, 2))
```
lambda 函数表达式
```
add = lambda x, y: x + y
print(add(1, 2))
```
####过滤器函数
filter 两个参数第一个参数可以是函数表达式，也可以设置成None,如果是一个函数的话，则将第二个可迭代的数据的每一个元素作为函数的参数进行运算。返回时true的值，如果第一个参数None的，则直接返回迭代器中是true的值
```
ls = filter(None,[1,2,True,False,0])
print(list(ls))
# [1, 2, True]

def evenNum(x):
    return False if x%2 else True

ls = filter(evenNum,range(10))
print(list(ls))
# [0, 2, 4, 6, 8]

ls = filter(lambda x:x%2==0,range(10))
print(list(ls))
# [0, 2, 4, 6, 8]
```





