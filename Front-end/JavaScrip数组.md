JS的数组使用中括号[]进行定界，中括号包裹的区域就是数组的元素。数组元素之间使用逗号隔开
####数组创建
数组也有其构造函数Array ,通过构造函数创建数组是，传入的参数为数组的初始化元素
var 变量名 = new Array(元素0，元素1,...)
```
var arr = new Array();//创建了一个空数组
var arr = new Array(1,2,3,4);//创建了一个数组，里面存放了4个数字
var arr = new Array(4);//创建了一个数组，长度为4,里面全是空值
```
[]直接创建
var 变量名 =[元素0，元素1]
```
var arr1 = []; //创建一个空数组
var arr2 = [1, 2 , 3, 4]; //创建一个包含4个数值的数组，多个数组项以逗号隔开
var arr3 = [4]; // 创建一个数组,元素只有1个,,,元素是4
```
## 数组的长度与下标

- 数组的长度 : 跟字符串一样,,,数组有一个length 属性,, 指数组中存放的元素的个数 ; 

  ```js
  var str1 = 'abc';
  console.log(str1.length);
  
  var arr = [1,3,5,8];
  console.log(arr.length);
  ```

- 数组的下标 : 因为数组有序的,有序的就应该有自己的序号,,而这个序号就是每个元素对应的下标,   **下标从0 开始 , 到 arr.length-1 结束**

  ```js
    var arr = ["zs", "ls", "ww"];
      arr[0];//下标0对应的值是zs
      arr[2];//下标2对应的值是ww
  
    var arr = ['zs','ls','ww'];
  
    //           0    1    2
  
    // 下标 :  0 开始     arr.length-1 结束     长度:3 arr.length
  
  ```
## 数组的取值
格式：数组名[下标]
  获取数组下标对应的那个值，如果下标不存在，则返回undefined。
下标范围 :  0 ~ arr.length-1
  ```js
 
  var arr = ["red", "green", "blue"];
  //
  打印 : arr[0];//red
  打印 : arr[2];//blue
  打印 : arr[3];//这个数组的最大下标为2,因此返回undefined
  ```
## 数组的赋值
  //格式：数组名[下标] = 值;
  //如果下标有对应的值，会把原来的值覆盖，
  ```js
  var arr = ["red", "green", "blue"];
  arr[0] = "yellow";//把red替换成了yellow
  // 如果下标不存在，会给数组新增一个元素。
  arr[3] = "pink";//给数组新增加了一个pink的值
  ```

##数组添加元素
```
arr[arr.length] = 值
arr.push(值)
```


## 遍历
数组遍历的基本语法：
```js
// 下标 :  0  arr.length-1
for(var i =0; i < arr.length; i++) {
	//数组遍历的固定结构
}
```
