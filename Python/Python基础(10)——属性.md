##属性定义
直接在类里定义
```
class Student(object):
    gender = 'male'
```
构造函数定义
```
class Student(object):
    gender = 'male'
    def __init__(self,name,age):
        self.name = name
        self.age = age

stu = Student("BX",26)
print(stu.name,stu.age,stu.gender)
# BX 26 male
```
## 私有属性
```
class Student(object):
    gender = 'male'
    def __init__(self,name,age,score):
        self.name = name
        self.age = age
        self.__score = score
    def getScore(self):
        return self.__score


stu = Student("BX",26,90)
print(stu.__dict__)
print(stu.getScore())
print(stu._Student__score)
# {'name': 'BX', 'age': 26, '_Student__score': 90}
# 90
# 90
```
1. 私有属性添加getter和setter方法
```
class Money(object):
    def __init__(self):
        self.__money = 0

    def getMoney(self):
        return self.__money

    def setMoney(self, value):
        if isinstance(value, int):
            self.__money = value
        else:
            print("error:不是整型数字")
```
2. 使用property升级getter和setter方法
```
class Money(object):
    def __init__(self):
        self.__money = 0

    def getMoney(self):
        return self.__money

    def setMoney(self, value):
        if isinstance(value, int):
            self.__money = value
        else:
            print("error:不是整型数字")
    money = property(getMoney, setMoney)
```
3. 使用property取代getter和setter方法
@property成为属性函数，可以对属性赋值时做必要的检查，并保证代码的清晰短小，主要有2个作用
将方法转换为只读
重新实现一个属性的设置和读取方法,可做边界判定
```
class Money(object):
    def __init__(self):
        self.__money = 0

    @property
    def money(self):
        return self.__money

    @money.setter
    def money(self, value):
        if isinstance(value, int):
            self.__money = value
        else:
            print("error:不是整型数字")
```

# 内建属性
子类没有实现`__init__`方法时，默认自动调用父类的。 如定义`__init__`方法时，需自己手动调用父类的 `__init__`方法

| 常用专有属性 | 说明 | 触发方式 |
| --- | --- | --- |
| `__init__` | 构造初始化函数 | 创建实例后,赋值时使用,在`__new__`后 |
| `__new__` | 生成实例所需属性 | 创建实例时 |
| `__class__` | 实例所在的类 | 实例.`__class__` |
| `__str__` | 实例字符串表示,可读性 | print(类实例),如没实现，使用repr结果 |
| `__repr__` | 实例字符串表示,准确性 | 类实例 回车 或者 print(repr(类实例)) |
| `__del__` | 析构 | del删除实例 |
| `__dict__` | 实例自定义属性 | `vars(实例.__dict__)` |
| `__doc__` | 类文档,子类不继承 | help(类或实例) |
| `__getattribute__` | 属性访问拦截器 | 访问实例属性时 |
| `__bases__` | 类的所有父类构成元素 | `类名.__bases__` |
##\_\_dict\_\_

##\_\_getattribute\_\_
获取属性
```
class Student(object):

    def __init__(self,name):
        self.name = name

    def __getattribute__(self, value):
        return super(Student,self).__getattribute__(value)
    def __setattr__(self, key, value):
        self.__dict__[key] = value

stu = Student("testStu")
print(stu.name)
```



