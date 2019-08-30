####反射机制
将类的各个组成部分封装成其他对象，称作反射机制，反射被称作是框架设计的灵魂。
####class
Java除了基本类型其他都是class，包括interface，String,Object Runnable,Exception
class的本质是数据类型Type
无继承关系的数据类型无法赋值
class/insterface的数据类型是class
每加载一个class,JVM就为其创建一个Class类型的实例，并关联起来
JVM持有的每个Class实例都指向一个数据类型
一个Clas实例包含
####获取字节码Class对象
同一个字节码文件(*.class)在一次程序运行过程中，只会被加载一次，无论通过哪一种方式获取的Class对象都是同一个。
1.Class.forName("全类名"):将字节码加载到内存，返回Class对象，主要用于配置文件，将类名定义在配置文件中，读取文件，加载类
```
Class c = Class.forName("domain.Person");
```
2 类名.class 通过类名的属性class获取，主要用于参数传递
```
Class c = Person.class;
```
3对象.getClass(): getClass()方法在object类中定义 ，主要用于对象获取字节码的方法。
```
            Person p = new Person();
            Class c = p.getClass();
```
####获取Class方法
```
getName()// 获取全类名
getSimpleName()
getPackage()
```
####获取成员变量
通过Class实例获取字段field信息:
```
getField(name):获取某个public的field(包括父类) 
getDeclaredField(name):获取当前类的某个field(不包括父类) 
getFields():获取所有public的field(包括父类) 
getDeclaredFields():获取当前类的所有field(不包括父类)
```
getFields 只能获取`public的方法`
```
            Class c = Person.class;
            Field[] fields = c.getFields();
            for (Field f :fields){
                System.out.println(f);
            }
```
getDeclaredFields()可以获取所有的成员变量 修饰符对其不影响。
```
            Class c = Person.class;
            Field[] fields = c.getDeclaredFields();
            for(Field f :fields){
                System.out.println(f);
            }
```
Field对象包含一个field的所有信息:
 ```
getName() 
getType() 
getModifiers() // 获取修饰符
```
获取和设置field的值:
```
get(Object obj)// 获取一个实例对象的该字段的值
set(Object, Object)// 设置一个实例对象的该字段的值
```
```
            Person p = new Person();
            p.setGender("male");
            Field f = Person.class.getField("gender");
            Object value = f.get(p);
            System.out.println(value);
            f.set(p,"female");
            System.out.println(p.getGender());
```
对私有变量的修改，暴力反射`setAccessible(true)`
```
            Person p = new Person();
            p.setName("xiaoming");
            Field f = Person.class.getDeclaredField("name");
            f.setAccessible(true);
            Object value = f.get(p);
            System.out.println(value);
            f.set(p,"xiaowang");
            System.out.println(p.getName());
```
####Constructor构造方法
```
Constructor<?> [] getConstructor()
Constructor<T> getConstructor(类<?>... parameterTypes)
```
构造函数创建对象`newInstance`
```
            Constructor constructor = Person.class.getConstructor(String.class,int.class);
            Object p = constructor.newInstance("xiaoming",20);
```
空参数构造可以使用class方法newInstance
####Methord成员方法
```
Methods []getMethods // 获取所有的public成员方法(包括父类方法)
Method getMethod(String name,类<?>...parameterTypes)
 Method[] getDeclaredMethods()
```
```
 Method[] methods = Person.class.getDeclaredMethods();
```
获取方法赋值`invoke`
```
            Method setName = Person.class.getMethod("setName",String.class);
            Person p = new Person();
            setName.invoke(p,"xiaoming");
```
获取成员方法名称
```
            Method[] methods = Person.class.getMethods();
            for(Method m :methods){
                System.out.println(m.getName());
            }
```
