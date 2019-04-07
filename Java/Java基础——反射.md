####class
Java除了基本类型其他都是class，包括interface，String,Object Runnable,Exception
class的本质是数据类型Type
无继承关系的数据类型无法赋值
class/insterface的数据类型是class
每加载一个class,JVM就为其创建一个Class类型的实例，并关联起来
JVM持有的每个Class实例都指向一个数据类型
一个Clas实例包含
####动态代理
####反射
反射的目的是当获得某个Object实例时，我们可以获得该Objct的class信息
```
getName()
getSimpleName()
getPackhage()
```
####fileld
通过Class实例获取字段field信息:
```
getField(name):获取某个public的field(包括父类) 
getDeclaredField(name):获取当前类的某个field(不包括父类) 
getFields():获取所有public的field(包括父类) 
getDeclaredFields():获取当前类的所有field(不包括父类)
```
Field对象包含一个field的所有信息:
 ```
getName() 
getType() 
getModifiers() // 获取修饰符
```
获取和设置field的值:
```
get(Object obj)// 获取一个实例的该字段的值
set(Object, Object)// 设置一个实例的该字段的值
```
