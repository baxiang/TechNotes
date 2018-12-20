## 1、数组的定义与特征
（1）数组元素的类型是唯一的，一个数组只能存储一种数据类型的数据。
（2）数组的长度是固定的，一旦数组初始化完成，数组所占有的内存空间将被固定。数组长度不可以改变。
（3）数组元素的类型既可以是基本类型 也是存储引用类型。

##2、数组的声明
```
type[] arrayName（推荐写法）
type arrayName []
```
##3、数组初始化
 (1)动态初始化 只指定长度，由系统给出初始化值，必须指定数组的长度值
```
int[] arr = new int[5]; 
```	
 (2)静态初始化 给出初始化值，不需要指定数组的长度，根据初始值的个数决定长度.
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
