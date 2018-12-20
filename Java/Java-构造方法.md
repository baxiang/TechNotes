构造方法是一种特殊的方法，构造主要特点是方法名称和当前类名称相同，如果类中没有定义有参数的构造方法，编译器会创建一个默认的缺省的构造方法，所以一个类至少存在一个构造方法。
``` 
 student s1 = new student();
```
注意：但是如果我们在类中创建了有参数的构造方法，编译器就不会创建那个默认的无参数构造方法
```
class TestClass{
	 public TestClass(String testArg) {
		// TODO Auto-generated constructor stub
	}
}
错误的： TestClass TestClass = new TestClass();
```
###构造器的作用
（1）创建对象 必须和new一起使用
（2） 完成对象的初始化操作
### 构造器的特点
  (1) 构造器的名称必须和当前类的名称相同
  (2) 返回值类型和当前构造器的类型相同 不写返回值，也不需要写明return；
### 自定义构造器
###  构造器的重载
