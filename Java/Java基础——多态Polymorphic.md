####方法的多态性
方法覆写:子类覆写父类的方法，Override
多态:
  针对某个类型的方法调用，其真正执行的方法取决于运行时实际类型的方法
  对某个类型调用方法，执行的方法可能是某个子类的覆写方法
  允许添加更多类型的子类来扩展功能
####关键字final
final修饰的方法不能被Override 
final修饰的类不能被继承
final修饰的field必须在创建对象时初始化
final修饰的字段初始化后不能重新赋值
####方法重载
（1）方法名称相同，方法参数个数,类型, 顺序不同。
方法重载要求两同一不同：同一个类中方法名称相同，参数列表不同，
####方法覆盖/覆写
当子类继承父类的所有可能被子类访问的成员方法时，如果子类的方法名和父类的方法名相同，那么子类不能继承父类的方法，此时子类覆写了父类方法。
(1)意味着拷贝父类的的方法 ，与父类的区别主要在方法体里面的内容不同。
####类的多态性
指的是发生在继承关系类中，子类和父类的转换。
向上转型：父类 父类对象 = 子类实例； //自动完成
向下转型：子类 子类对象 = （子类）父类实例。 //强制完成
```
public class Person {

    public void run(){
        System.out.println("Person run");
    }
}

```
student
```
public class Student  extends Person{
    @Override
    public void run() {
        System.out.println("Student run");
    }
}

```
main
```
public class Main {

    public static void main(String[] args) {
       Person s = new Student();
       s.run();
    }
}
```
####Object重要方法
toString 把实例字符串化
equals 判断两个实例是否逻辑相等
hashCode 计算一个实例的哈希值
