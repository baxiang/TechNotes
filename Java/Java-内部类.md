## 普通内部类
（1）普通内部类可以访问外部类的所有成员（包括外部类中的private成员,可以看做外部类的一个成员。
（2）普通内部类与外部类的实例联系，因此不能在其内部定义静态成员
```
public class TestInner {
    int a = 10;
    static String s = "baxiang";
    private String privateStr = "privateStr";
	class InnerClass{
		int m = 20;
		void printTest(){
			System.out.println(a);
			System.out.println(s);
			System.out.println(privateStr);
		}
	}
}
```
## 静态内部类
静态内部类无法直接访问外部类的非静态成员，只能访问外部类的静态成员
```
public class TestInner {
    int a = 10;
    static String s = "baxiang";
    private String privateStr = "privateStr";
    static void test(){
    }
	static class StaticInnerClass{
		void printTest(){
			System.out.println(a);// 错误无法访问
			System.out.println(s);
			System.out.println(privateStr);// 错误无法访问
	        test();
		}
		
	}
	public static void main(String[] args) {
		// TODO Auto-generated method stub
	}

}
```
## 局部内部类
局部类是指定义在一个方法体内的类，局部内部类可以访问外部类的所有成员、方法的局部变量、方法的形参
```
void innerClassFun(){
		 int b = 10;
		class innerClass{
			 void testFun(){
				 System.out.println(a);
		           System.out.println(s);
		           System.out.println(privateStr);
			    }
		};
```
## 匿名内部类
没有名字的内部类就是匿名内部类
(1)不能在匿名类中声明构造函数
(2)不能在匿名类中声明静态成员和接口，除非静态成员被final修饰。
```
class ClassTest{
	 void test1(){
		 
	 }
	 void test2(){
		 
	 }
}
public class TestInner {
     void fun(){
    	 new ClassTest(){
    		 void test1(){
    			 super.test1();
    		 }
	     }; 
     }
	public static void main(String[] args) {
		// TODO Auto-generated method stub 
	}
}
```
