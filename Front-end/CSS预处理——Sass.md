####概述
官方地址[https://www.sass.hk/](https://www.sass.hk/)
####css预处理工具
CSS 预处理器用一种专门的编程语言，进行 Web 页面样式设计，然后再编译成正常的 CSS 文件，以供项目使用。CSS 预处理器为 CSS 增加一些编程的特性，无需考虑浏览器的兼容性问题
解决的问题
嵌套规则：通过花括号的方式解决复杂的css父子样式嵌套问题。
变量规则：通过变量将公告样式抽离，减少冗余css代码。
提交逻辑 ：可以像高级语言一样编写逻辑性的css代码
####Sass 和 SCSS 区别
Sass 和 SCSS 其实是同一种东西，我们平时都称之为 Sass，两者之间不同之处有以下两点：
文件扩展名不同，Sass 是以“.sass”后缀为扩展名，而 SCSS 是以“.scss”后缀为扩展名
语法书写方式不同，Sass 是以严格的缩进式语法规则来书写，不带大括号({})和分号(;)，而 SCSS 的语法书写和我们的 CSS 语法书写方式非常类似。
####Sass 和 CSS 差别：
Sass 和 CSS 写法的确存在一定的差异，由于 Sass 是基于 Ruby 写出来，所以其延续了 Ruby 的书写规范。在书写 Sass 时不带有大括号和分号，其主要是依靠严格的缩进方式来控制的。如：
Sass写法：
```
body
  color: #fff
  background: #f36
```
而在 CSS 我们是这样书写：
```
body{
  color:#fff;
  background:#f36;
}
```
####SCSS 和 CSS 差别：
SCSS 和 CSS 写法无差别，这也是 Sass 后来越来越受大众喜欢原因之一。简单点说，把你现有的“.css”文件直接修改成“.scss”即可使用。

####安装
安装之sass前需要查看是否安装了ruby
```
ruby -v
```
安装sass
```
sudo gem install sass
```
判断是否安装成功
```
$ sass -v
Ruby Sass 3.7.4
```
vue中使用
```
npm install --save-dev sass-loader
npm install --save-dev node-sass
```


编译scss 文件
```
//单文件转换命令
sass input.scss output.css

//单文件监听命令
sass --watch input.scss:output.css

//如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
sass --watch app/sass:public/stylesheets
```
只能一次性编译。每次个性保存“.scss”文件之后，都得重新执行一次这样的命令。如此操作太麻烦，其实还有一种方法，就是在编译 Sass 时，开启“watch”功能，这样只要你的代码进行任保修改，都能自动监测到代码的变化，并且给你直接编译出来：
```
sass --watch <要编译的Sass文件路径>/style.scss:<要输出CSS文件路径>/style.css
```
####注释
1、开头使用 ”/* ”，结属使用 ”*/ ”,会在编译出来的 CSS 显示
2、使用“//”,编译出来的 CSS 中不会显示
```
//定义一个占位符

%mt5 {
  margin-top: 5px;
}

/*调用一个占位符*/

.box {
  @extend %mt5;
}
```
####变量：
以美元符号“$”开头。
![](https://upload-images.jianshu.io/upload_images/143845-95dbfaa0bee776b2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Sass 的变量包括三个部分：
1.  声明变量的符号“$”
2.  变量名称
3.  赋予变量的值

变量定义之后可以在全局范围内使用。
```
$fontSize: 12px;
body{
    font-size:$fontSize;
}
```
编译后的css代码：
```
body{
    font-size:12px;
}
```
####默认变量!default
sass 的默认变量仅需要在值后面加上 !default 即可。
```
$baseLineHeight:1.5 !default;
body{
    line-height: $baseLineHeight; 
}
```
编译后的css代码：
```
body{
    line-height:1.5;
}
```
sass 的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可。
 ```
$baseLineHeight: 2;
$baseLineHeight: 1.5 !default;
body{
    line-height: $baseLineHeight; 
}
```
编译后的css代码：
```
body{
    line-height:2;
}
```
可以看出现在编译后的 line-height 为 2，而不是我们默认的 1.5。默认变量的价值在进行组件化开发的时候
####选择器嵌套
假设我们有一段这样的结构：
```
<header>
<nav>
    <a href=“##”>Home</a>
    <a href=“##”>About</a>
    <a href=“##”>Blog</a>
</nav>
<header>
```
想选中 header 中的 a 标签，在写 CSS 会这样写：
```
nav a {
  color:red;
}

header nav a {
  color:green;
}
```
那么在 Sass 中，就可以使用选择器的嵌套来实现：
```
nav {
  a {
    color: red;

    header & {
      color:green;
    }
  }  
}
```
####属性嵌套
Sass 中还提供属性嵌套，CSS 有一些属性前缀相同，只是后缀不一样，比如：border-top/border-right，与这个类似的还有 margin、padding、font 等属性。假设你的样式中用到了：
```
.box {
    border-top: 1px solid red;
    border-bottom: 1px solid green;
}
```
在 Sass 中我们可以这样写：
```
.box {
  border: {
   top: 1px solid red;
   bottom: 1px solid green;
  }
}
```
字体大小
```
.box {
  font: {
   size: 12px;
   weight: bold;
  }  
}
```
####伪类嵌套

其实伪类嵌套和属性嵌套非常类似，只不过他需要借助`&`符号一起配合使用。我们就拿经典的“clearfix”为例吧：
```
.clearfix{
&:before,
&:after {
    content:"";
    display: table;
  }
&:after {
    clear:both;
    overflow: hidden;
  }
}
```
编译出来的 CSS：
```
clearfix:before, .clearfix:after {
  content: "";
  display: table;
}
.clearfix:after {
  clear: both;
  overflow: hidden;
}

```
#####混合宏
不带参数混合宏：
在 Sass 中，使用`@mixin`来声明一个混合宏。如：
```
@mixin border-radius{
    -webkit-border-radius: 5px;
    border-radius: 5px;
}
```
其中 @mixin 是用来声明混合宏的关键词，有点类似 CSS 中的 @media、@font-face 一样。border-radius 是混合宏的名称。大括号里面是复用的样式代码。
带参数混合宏：
除了声明一个不带参数的混合宏之外，还可以在定义混合宏时带有参数，如：
```
@mixin border-radius($radius:5px){
    -webkit-border-radius: $radius;
    border-radius: $radius;
}
```
复杂的混合宏：

上面是一个简单的定义混合宏的方法，当然， Sass 中的混合宏还提供更为复杂的，你可以在大括号里面写上带有逻辑关系，帮助更好的做你想做的事情,如：
```
@mixin box-shadow($shadow...) {
  @if length($shadow) >= 1 {
    @include prefixer(box-shadow, $shadow);
  } @else{
    $shadow:0 0 4px rgba(0,0,0,.3);
    @include prefixer(box-shadow, $shadow);
  }
}
```
这个 box-shadow 的混合宏，带有多个参数，这个时候可以使用“ … ”来替代。简单的解释一下，当 $shadow 的参数数量值大于或等于“ 1 ”时，表示有多个阴影值，反之调用默认的参数值“ 0 0 4px rgba(0,0,0,.3) ”。

####@include调用混合宏
在 Sass 中通过 @mixin 关键词声明了一个混合宏，那么在实际调用中，其匹配了一个关键词`@include`来调用声明好的混合宏。例如在你的样式中定义了一个圆角的混合宏“border-radius”:
```
@mixin border-radius{
    -webkit-border-radius: 3px;
    border-radius: 3px;
}
```
在一个按钮中要调用定义好的混合宏“border-radius”，可以这样使用：
```
button {
    @include border-radius;
}
```
这个时候编译出来的 CSS:
```
button {
  -webkit-border-radius: 3px;
  border-radius: 3px;
}
```
####混合宏缺点
混合宏可以实现复用重复代码块。但其最大的不足之处是会生成冗余的代码块。比如在不同的地方调用一个相同的混合宏时。如：
```
@mixin border-radius{
  -webkit-border-radius: 3px;
  border-radius: 3px;
}

.box {
  @include border-radius;
  margin-bottom: 5px;
}

.btn {
  @include border-radius;
}
```
示例在“.box”和“.btn”中都调用了定义好的“border-radius”混合宏。先来看编译出来的 CSS：
```
.box {
  -webkit-border-radius: 3px;
  border-radius: 3px;
  margin-bottom: 5px;
}

.btn {
  -webkit-border-radius: 3px;
  border-radius: 3px;
}
```
Sass 在调用相同的混合宏时，并不能智能的将相同的样式代码块合并在一起。这也是 Sass 的混合宏最不足之处。
####继承@extend
Sass中是通过关键词 `@extend`来继承已存在的类样式块，从而实现代码的继承。如下所示：
```
//SCSS
.btn {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}

.btn-primary {
  background-color: #f36;
  color: #fff;
  @extend .btn;
}

.btn-second {
  background-color: orange;
  color: #fff;
  @extend .btn;
}
```
编译出来之后：
```
//CSS
.btn, .btn-primary, .btn-second {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}

.btn-primary {
  background-color: #f36;
  color: #fff;
}

.btn-second {
  background-clor: orange;
  color: #fff;
}
```
从示例代码可以看出，在 Sass 中的继承，可以继承类样式块中所有样式代码，而且编译出来的 CSS 会将选择器合并在一起，形成组合选择器：
```
.btn, .btn-primary, .btn-second {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}
```
####占位符 %
Sass 中的占位符 %placeholder 功能是取代以前 CSS 中的基类造成的代码冗余的情形。因为 %placeholder 声明的代码，如果不被 @extend 调用的话，不会产生任何代码。来看一个演示：
```
%mt5 {
  margin-top: 5px;
}
%pt5{
  padding-top: 5px;
}
```
这段代码没有被 @extend 调用，他并没有产生任何代码块，只是静静的躺在你的某个 SCSS 文件中。只有通过 @extend 调用才会产生代码：
```
//SCSS
%mt5 {
  margin-top: 5px;
}
%pt5{
  padding-top: 5px;
}

.btn {
  @extend %mt5;
  @extend %pt5;
}

.block {
  @extend %mt5;

  span {
    @extend %pt5;
  }
}
```
编译出来的CSS
```
//CSS
.btn, .block {
  margin-top: 5px;
}

.btn, .block span {
  padding-top: 5px;
}
```

![image.png](https://upload-images.jianshu.io/upload_images/143845-15508664c444761b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####插值#{}
语法格式
```
#{$变量名称}
```
```
$width:300px;
$height:200px;
$avatar:'avatar.png';
.div1{
  width: $width;
  height: $height;
  background-image: url("./img/#{$avatar}");
}
```
![](https://upload-images.jianshu.io/upload_images/143845-04e7eecc19f71f35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
