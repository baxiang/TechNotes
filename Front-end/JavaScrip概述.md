##概述
JavaScript 是一种运行在 客户端  的 解释性语言
**网页三部分**
HTML：控制网页的 **结构**
CSS：控制网页的 **样式**
JavaScript：**控制网页的行为** 
##javascript的组成
![javascript.png](https://upload-images.jianshu.io/upload_images/143845-f6e8b01bc99f30a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 ECMAScript - JavaScript的核心，描述了语言的基本语法和数据类型，ECMAScript是一套标准，定义了一种语言的标准,与具体实现无关
  DOM：一套操作网页元素的API
 BOM：一套操作浏览器功能的API
## JavaScript书写位置
 第一种 : 写在`script`标签中

```html
<script>
  alert('Hello World!');
</script>
```
 第二种 : 引入一个js文件

```html
<script src="main.js"></script>
```
## 输出语句 
弹框输出
alert : 警告框/confirm : 确认框/prompt : 输入框
  ```js
  //alert会弹出一个警告框
  alert("hello world");
  //confirm弹出一个确定框
  confirm("你确定？");
  //prompt:弹出一个输入框，可以输入值
  prompt("请输入用户名");
  ```
document.write : 网页中写入内容

  ```js
  //可以识别标签
  document.write("hello world");
  document.write("<h1>hello world</h1>");
  ```

控制台输出
  ```js
  //F12打开控制台，在console中可以看到打印的信息
  console.log("hello word");
  ```



