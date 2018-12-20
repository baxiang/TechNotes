####定义类
定义类使用class关键字，class 后面紧跟着类名称，类名称通常首字母大写，类名称后面（object）代表当前的类的继承自object类。类主要包含属性和方法

具体定义stduent类
```
class Student(object):
    def getName(self):
        print("获取姓名")
    
    def setName(self):
        print("设置姓名")
```
#### 实例对象
```
对象名 = 类名()
```
```
class Student:
    def getName(self):
        print("获取姓名")

    def setName(self):
        print("设置姓名")

s = Student()
s.getName()
s.setName()
# 获取姓名
# 设置姓名
```
####构造函数
\__init\_\_()方法是在创建对象后，就立刻被默认调用了，一个类中可以定义多个构造函数，但是实例化时只实例化最后一个构造方法，后面的构造方法会覆盖前面的构造方法。
```
 def __init__(self):
        self.Name = ""
        self.age = 0
```
####属性
在属性名前面加了2个下划线'__'，则表明该属性是私有属性，否则为公有属性
```
class Student:
    def __init__(self,name):
        self.__name = name

    def getName(self):
        return self.__name

    def setName(self,name):
        self.__name = name


s = Student("init name")
print(s.getName())
s.setName("new name")
print(s.getName())
```
##私有属性
它是以属性命名方式来区分，如果在属性名前面加了2个下划线'__'，则表明该属性是私有属性，否则为公有属性（方法也是一样，方法名前面加了2个下划线的话表示该方法是私有的，否则为公有的）。
```
class People(object):

    def __init__(self,name):
        self.__name = name
    def setName (self, newName):
        if len(newName)>=5:
            self.__name = newName
        else:
            print("error 名字长度需要大于或者等于5")
    def getName(self):
        return self.__name

xiaoming = People("xiaoming")
print(xiaoming.getName())
xiaoming.setName("wangwu")
print(xiaoming.getName())
```
## 类属性
在前面的例子中我们接触到的就是实例属性（对象属性），顾名思义，类属性就是类对象所拥有的属性，它被所有类对象的实例对象所共有，在内存中只存在一个副本，这个和C++中类的静态成员变量有点类似。对于公有的类属性，在类外可以通过类对象和实例对象访问
##类方法
是类对象所拥有的方法，需要用修饰器@classmethod来标识其为类方法，对于类方法，第一个参数必须是类对象，一般以cls作为第一个参数（当然可以用其他名称的变量作为其第一个参数，但是大部分人都习惯以'cls'作为第一个参数的名字，就最好用'cls'了），能够通过实例对象和类对象去访问。
## 类方法和静态方法
```
class People(object):
       country ="China"

       @classmethod
       def getCountry(cls):
           return cls.country

       @staticmethod
       def language():
           return "chinese"

p = People()
print(p.getCountry())
print(People.getCountry())
print(People.language())
```
## 继承

```
class Person(object):
    def __init__(self,name,gender,age):
        self.name = name
        self.gender = gender
        self.__age = age
    @property
    def get_age(self):
        return self.__age



class Student(Person):

    def __init__(self,name,gender,age,score):
        super(Student,self).__init__(name,gender,age)
        self.score = score



stu = Student("BX","m",26,90)
print(stu.__dict__)
print(stu.get_age)
#{'name': 'BX', 'gender': 'm', '_Person__age': 26, 'score': 90}
#26
```
####私有方法
私有方法和私有属性一样 都是__开头作为私有方法

####多态
当子类和父类有相同的方法时，子类的方法会覆盖父类的方法，当子类对象只会调用子类的方法，这就是多态
```
class Animal(object):
    def cry(self):
        print("动物叫")

class Dog(Animal):
    def cry(self):
        print("旺旺")


class Cat(Animal):
    def cry(self):
        print("喵喵")

a = Animal()
a.cry()
d = Dog()
d.cry()
c = Cat()
c.cry()
```
