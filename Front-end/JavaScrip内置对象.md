## Math对象
Math对象中封装很多与数学相关的属性和方法。
- π
```
Math.PI
```
- 最大值/最小值
```
Math.max();
Math.min();
  ```

- 取整  [1.1 , 1.9,  -1.1 , -1.9  ,  1.5]

```javascript
  Math.ceil();//天花板，向上取整
  Math.floor();//地板，向下取整
  Math.round();//四舍五入，如果是.5，则取更大的那个数
```
- 随机数

```javascript
  Math.random();//返回一个[0,1)之间的数，能取到0，取不到1
```

- 绝对值    (abs absolute 绝对)

```javascript
  Math.abs();//求绝对值
 ```

- 次幂和平方    (pow power 幂   sqrt:开方 )

 ```javascript
  Math.pow(num, power);//求num的power次方
  Math.sqrt(num);//对num开平方
```


## Date对象
Date对象用来处理日期和时间

- 创建一个日期对象

```javascript
  var date = new Date();//使用构造函数创建一个当前时间的对象
  var date = new Date("2017-03-22");//创建一个指定时间的日期对象
  var date = new Date("2017-03-22 00:52:34");//创建一个指定时间的日期对象
  var date = new Date(2017, 2, 22, 0, 52, 34);
  var date = new Date(1523199394644);//参数：毫秒值
  
  Date构造函数的参数
  1. 毫秒数 1498099000356		new Date(1498099000356)
  2. 日期格式字符串  '2015-5-1'	 new Date('2015-5-1')
  3. 年、月、日……				 var date = new Date(2017, 2, 22, 0, 52, 34);月份从0开始
```

- 日期格式化

```javascript
  date.toLocalString();//本地风格的日期格式
  date.toLocaleDateString(); // 获取日期
  date.toLocaleTimeString(); // 获取时间
```

- 获取日期的指定部分 
```javascript
  getMilliseconds();//获取毫秒值
  getSeconds();//获取秒
  getMinutes();//获取分钟
  getHours();//获取小时
  getDay();//获取星期，0-6    0：星期天
  getDate();//获取日，即当月的第几天
  getMonth();//返回月份，注意从0开始计算，这个地方坑爹，0-11
  getFullYear();//返回4位的年份  如 2016
```
- 格式化当前时间
```
 function getD() {
    var date = new Date();
    var year = date.getFullYear();
    var month =  date.getMonth() + 1;
    var day = date.getDate();
    var h = date.getHours();
    var m = date.getMinutes();
    var s = date.getSeconds();
    function addZero(num) {
       return num < 10 ? '0'+num : num;
    }
    return year+ '-'+ addZero(month) +'-'+  addZero(day) +' '+  addZero(h) +':' +  addZero(m) + ':' +  addZero(s);
  }
  ```

- 时间戳
  ```js
  var date = +new Date();//1970年01月01日00时00分00秒起至现在的总毫秒数
  //思考
  //如何统计一段代码的执行时间？  打印10000次
  // 开始 
  var start = +new Date();
  for ( var i = 1 ; i <= 1000 ; i++) {
  
      console.log(i);
  }
  // 结束
  var end = +new Date();
  console.log('时间戳 :' + (end-start));
  ```
## Array对象
####join
将数组的值拼接成字符串,并且返回字符串
  ```javascript
  var arr = [1,2,3,4,5];
  arr.join();//不传参数，默认按【,】进行拼接
  arr.join("");//按【"】进行拼接
  arr.join("-");//按【-】进行拼接
  ```
####增加
 ```javascript
  var arr = ['1','2','3']
  array.push(元素);//从后面添加元素，返回新数组的length
  array.unshift(元素);//从数组的前面的添加元素，返回新数组的长度
```
####删除
```
 array.pop();//从数组的后面删除元素，返回删除的那个元素
 array.shift();//从数组的最前面删除元素，返回删除的那个元素
```
####reverse
翻转数组
```
   var array = [1,2,3,4,5]
   console.log(array.reverse())
```
####sort
数组的排序，默认按照 字母/首字符 顺序排序
  ```javascript

  var arr1 =  ['a','d','b','c'];
  var arr2 = [3, 6, 1, 5, 10, 2,11];
  
  //sort方法可以传递一个函数作为参数，这个参数用来控制数组如何进行排序
  arr.sort(function(a, b){
    //如果返回值>0,则交换位置
    return a - b;
  });
  
  ```
####concat
concat：数组合并，不会影响原来的数组，会返回一个新数组。

 ```javascript
  var newArray = array.concat(array2);
```
 ####slice
 slice:截取出来，既然是截取`出来`,肯定要有个东西接收原来的数组不受影响，
 - slice() 全部截取出来
- slice(begin) 从第begin往后截取出来
- slice(begin, end) 从第begin开始删除,,不包括end   [start, end)
```
  var arr = ['zs','ls','ww','zl','xmg'];
  var newArray = array.slice(begin, end);
 ``` 
 ####splice
 数组任意地方删除或者添加元素   start 开始 deletedCount 删除个数
```
splice(start, deletedCount)   删除元素
```
删除+添加,  第三个参数是在原来删除的位置上新加几个元素
```
 splice(start, deletedCount , item) 
```
在某个位置新加元素
```
   splice(start, 0 , item)   
  ```
####查找
indexOf方法用来查找数组中某个元素 `第一次`出现的位置，如果找不到，返回-1
```
 array.indexOf(search, [fromIndex]);
```
lastIndexOf()方法用来查找数组中某个元素 `最后一次`出现的位置,如果找不到，返回-1
```
  array.lastIndexOf(search, [fromIndex]);
```
####清空
-  array.splice(0,array.length);//删除数组中所有的元素
- array.length = 0;//直接修改数组的长度
- array = [];//将数组赋值为一个空数组

## 基本包装类型
JavaScript还提供了三个特殊的引用类型：String/Number/Boolean。
基本包装类型：把基本类型包装成复杂类型。

```javascript
var str = “abc”;
var result = str.indexOf(“a”);
//发生了三件事情
1. 把简单类型转换成复杂类型：var s = new String(str);
2. 调用包装类型的indexOf方法：var result = s.indexOf(“a”);
3. 销毁刚刚创建的复杂类型

总结 : js为了我们使用方便，浏览器允许使用简单类型直接调用方法，会自动把简单类型转换成复杂类型。
```
## Number对象
 Number对象是数字的包装类型，数字可以直接使用这些方法

```javascript
toFixed(2)//保留2位小数
toString();//转换成字符串
```

## Boolean对象
 Boolean对象是布尔类型的包装类型。

```javascript
toString( );//转换成字符串
```

##String
注意 : 操作字符串的方法都不会改变原来的字符串,,所以需要返回
####查找

indexOf:获取某个字符串第一次出现的位置，如果没有，返回-1
lastIndexOf:获取某个字符串最后一次出现的位置。如果没有，返回-1

####去除

  trim();//去除字符串两边的空格，内部空格不会去除

####大小写转换

toUpperCase：全部转换成大写字母
toLowerCase：全部转换成小写字母

####拼接
可以用concat，用法与数组一样，但是字符串拼串我们一般都用+
####截取

slice ：截取出来 从start开始，end结束，并且取不到end。 `和 substring一样`
substring ：从start开始，end结束，并且取不到end
substr ： ：从start开始，截取length个字符

####切割
split:将字符串分割成数组（很常用）
```
  var str = "张三,李四,王五";
  var arr = str.split(",");
 ```
####替换

  var str = 'abcedabc'
  str = str.replace(/a/g,'c')
  replace(searchValue, replaceValue)  //  replace(old, new)
  //参数：searchValue:需要替换的值    replaceValue:用来替换的值

####遍历
```js
  var  str1 = 'abcoefoxyozzopp'
  for ( var i = 0 ; i < str1.length ; i++) {
      console.log(str1[i]);
  }
  str[0] == str.charAt(0)
  ```


