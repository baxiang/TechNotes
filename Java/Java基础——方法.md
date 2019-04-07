####方法重载（Overload）
方法重载完成相同的功能,且多个方法的方法名相同，但是各自的参数不同。
方法重载主要依靠参数类型和数量区分。
方法重载返回值类型应该相同。
####构造方法
构造方法是一种特殊的方法，构造主要特点是:
1方法名称和当前类名称相同。
2如果类中没有定义有参数的构造方法，编译器会创建一个默认的缺省的构造方法，所以一个类至少存在一个构造方法。
3构造方法没有返回值，也没有void
4构造方法只能与new结合使用
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
####构造器的重载
```

    public class User{
        private String name;
        private int age;
        private Date birthday;
        public User(String n,int a,Date b){
            name = n;
            age = a;
            birthday = b;
        }
        public User(String n,int a){
            name = n;
            age = a;
        }
        public User(String n){
            name = n;
        }
    }
```
