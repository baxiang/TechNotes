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
