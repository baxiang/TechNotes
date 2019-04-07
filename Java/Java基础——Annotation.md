注解相当于一种标记，在程序中加了注解就等于为程序打上了某种标记，javac编译器，开发工具和其他程序可以用反射来了解你的类及各种元素上有无何种标记，看你有什么标记，就去干相应的事。标记可以加在包，类，字段，方法，方法的参数以及局部变量上。
####@Override
子类覆盖父类方法或者实现接口的方法
```
public class User {

    private String name;
    private int age;

    public User(String n,int a){
        this.name = n;
        this.age = a;
    }

    @Override
    public String toString() {
        return "current user name:"+this.name+" age:"+this.age;
    }
}
public static void main(String[] args) {
       User user = new User("tom",20);
       System.out.println(user);

    }
```
####@Deprecated
表示API已经过时
```
 @Deprecated
    public User(String n,int a){
        this.name = n;
        this.age = a;
    }
```
####SuppressWarnings
抑制编译警告，若是不想看到这些警告
```
@SuppressWarnings("deprecation")
    public static void main(String[] args) {
       User user = new User("tom",20);
       System.out.println(user);

    }
```
####注解分类
源码注解：注解只在源码中存在。编译成.class文件就不存在。
编译时注解：注解在源码和.class文件中都存在
运行时注解：在运行阶段还起作用，甚至会影响运行逻辑的注解
