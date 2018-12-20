JavaScrip是弱类型语言，在声明的变量的时无需指定任何数据类型。
##变量声明
声明一个变量时，需要使用关键字var
##变量赋值
```
   <script>
       var message = "好好学习 天天向上"
       document.write(message)
   </script>
```
##全部变量和局部变量
对应的是两个范围分别是全局作用域和局部作用域，一般我们使用函数来声明局部作用域，全局作用域是一个Javascrip文件或者HTML。
```
   <script>
       var globeMess = "全局作用域"
       function foo() {
           var localMess = "局部作用域"
           document.write('<p>',localMess,'</p>')
           document.write('<p>',globeMess,'</p>')
       }
       foo()
       document.write('<p>',globeMess,'</p>')
      // document.write(localMess) 错误
   </script>
```
