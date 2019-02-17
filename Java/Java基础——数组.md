####特征
（1）数组元素的类型是唯一的，一个数组只能存储一种数据类型的数据。
（2）数组的长度是固定的，一旦数组初始化完成，数组所占有的内存空间将被固定。数组长度不可以改变。
（3）数组元素的类型既可以是基本类型 也是存储引用类型。
####声明
```
数据类型[] 数组变量名（推荐写法）
数据类型 数组变量名 []
```
Java是面向对象的语言，数据类型[]可以看成是一个整体类型，即数组类型，第二种是C 语言数组的声明方式。
```
int[] intArr;
String[] stringArr;
```
####初始化
数组初始化的过程就是为数组的元素分配内存空间，并且为每个元素赋初始值
 **(1)动态初始化 **
动态初始化使用new运算符分配指定长度的内存空间，只指定长度，由系统给出初始化值，注意必须指定数组的长度值
```
new 数据类型[数组长度]
```
eg
```
        int[] intArr = new int[2];
        intArr[0] = 1;
        intArr[1] = 2;
        String[] stringArr = new String[3];
        stringArr[0] = "Hello world";
        stringArr[1] = "Hello Java";
        stringArr[2] = "Hello Python";
```	
 **(2)静态初始化**
```
类型[] 数组名称 = new 类型[]{元素,元素,....}
类型[] 数组名 = {元素,元素,....}
```
 给出初始化值，不需要指定数组的长度，根据初始值的个数决定长度.
```	
 int[] arr = new int[]{1,2,3,4,5}; 
 int[] arr = {1,2,3,4,5};
```
数组的初始化具体示例用法
```
public class Main {

    public static void main(String[] args){
        int [] arr = new int[10];// 声明数组
        for(int i =0;i<arr.length;i++){
            arr[i]=i;
        }
        int []numbers = new int[]{100,90,80};
        for(int i =0;i<numbers.length;i++){
            System.out.println(numbers[i]);
        }
        for(int num :numbers){
            System.out.println(num);
        }
    }
}
```
#####ArrayIndexOutOfBoundsException
每个数组的索引都有一个范围，即0——length-1,在访问数组的元素，不能超过数组范围，否则引起ArrayIndexOutOfBoundsException
```
public static void main(String[] args) {
       int [] arr ={1,2,3,4};
       System.out.println(arr[4]);
    }
```
####NullPointerException
空指针异常，使用变量引用一个数组的时候，必须执行一个有效的数组对象，如果该变量值为null,则意味没有指向任何数组，此时通过变量名称访问数组元素会出现空指针异常
```
public static void main(String[] args) {
       int [] arr ={1,2,3,4};
       System.out.println(arr[0]);
       arr = null;
       System.out.println(arr[0]);
    }
```
