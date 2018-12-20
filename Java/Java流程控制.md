## 循环结构
###1for循环
```
for(初始化;布尔表达式;更新){
 //需要执行的代码;
}
```
ex:
```
public static void main(String[] args){
 for(int i = 0; i < 100; i++){
    System.out.println("第"＋(i＋1)+"循环");
 }
 //for循环另外一种表达形式
 int i = 0;
 for(;i < 10;i++){
   System.out.println("当前i值:"＋i);
 }
}
```
###2for-each循环
```
for(声明局部变量 :集合容器){
 //需要执行的代码;
}
```
 ex:
```
public static void main(String[] args){
 int[] items = {1,2,3,4,5,6,7,8,9,10};
 for(int item:items){
   System.out.println("元素是:"+ item);
 }
}
```
###3while循环
```
//只要布尔表达式结果为true,则会一直执行循环内容
  while(布尔表达式){
    //循环内容；
  }
```
while循环的特点是先判断布尔表达式是否为真，如果为真，继续执行循环体，否则跳过循环
```
public static void main(String[] args){
  int a = 10;
  while(a > 0){
      a--;
      System.out.println("当前a="+a);
  }
}
```
###do..while循环
```
do{
  //执行代码;
}while(布尔表达式);
```
布尔表达式在循环体的后面，所以语句块在检测布尔表达式之前已经执行了。 如果布尔表达式的值为true，则语句块一直执行，直到布尔表达式的值为false。
```
do-while ex:
public static void main(String[] args){
   int a = 10;
   do{
      a--;
      System.out.println("当前a="+a);
   }while(a > 0);
}
```
##分支结构
###1.if...else条件判断

```
if(布尔表达式){
   //如果为真，布尔表达式为true;
   //可执行代码;
 } else {
   //如果为假，布尔表达式为false;
   //可执行代码;
 }
```
ex
```
public static void main(String[] args){
   for(int i = 1;i < 10;i++){
     if(i%2 == 0){
        System.out.println("i="+i);
     } else {
        System.out.println("i="+i);
     }
   }
}
```
###2switch...case...default分支判断
```
switch(变量){ 
    case value : 
         //可执行代码 
         break; //可选 
    case value : 
         //可执行代码 
         break; //可选 
       //你可以有任意数量的case语句 
    default : //可选 
         //可执行代码 
}
```
ex:
```
public static void main(String args[]){ 
   int a = 1; 
   switch(a) {
        case 0 : 
           System.out.println("a=0"); 
           break; 
        case 1 : 
        case 2 :
           System.out.println("a=2"); 
           break; 
        case 3 :
           System.out.println("a=3"); 
       default : 
           System.out.println("输入a＝"＋a); 
   } 
}
```
