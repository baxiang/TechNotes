####字体大小   
font-size  设置字体的大小 ,px 是一个单位，代表屏幕的上的像素，在css大多数数值都需要添加单位
```html
font-size: 12px;  
```
####字体粗细 
font-weight 设置字体的粗细 ，取值：默认(normal) 、加粗(bold)、 100 - 900
```html
font-weight:bold
```
因为字体在初始设计的时候就没有设置太多的粗细标准，用数字设置的时候，只有在400和700会产生变化，在实际工作中 用的最多的就是normal(400） bold（700）
####字体风格
font-style 设置字体的风格(样式)
取值：normal 默认 显示标准的字体样式 italic 字体倾斜
```html
font-style:italic;
```
####字体类型
font-family 设置不同的字体,取值：宋体、微软雅黑、黑体等等。
```html
font-family:"宋体";
```
字体可以写多个，中间用逗号隔开，浏览器会从左到右依次解析，直到识别出当前电脑安装的字体则直接使用,字体名称中如果有空格 # $ 这种特殊字符的时候需要添加上引号 中文字体也需要添加引号,了解：http://code.ciaoca.com/style/cssfont2unicode/

```html
font: font-style  font-weight  font-size/line-height  font-family;
```
不建议修改顺序  并且不需要设置的属性可以不写  但是font-size和font-family必须指定，否则将不起作用
####行高
行高控制的是文字与文字之间的上下距离 （行距）
```
line-height:值;
```
1. 值的取值是像素
2. **小技巧：如果将标签的高度和行高设置成一样，那么这个标签里面的文字可以在这个标签里面垂直居中
3. 两者结合使用可以让单行文字在标签内部水平垂直居中
####文字的对齐 
text-align:值;  
```
取值：left right center
该属性控制的是标签  “内部的文字”  水平居中
```
####首行缩进
```
text-indent:值;
取值可以是像素，也可以是em  推荐写法：text-indent:2em;
```

字体下划线和删除线
```
text-decoration:值;
取值：underline 下划线  line-through 删除线 none 去掉多余的样式
```






