##字符串
|元字符/元符号|匹配情况|
|:------:|:-------:|
|.|匹配除换行符外的任意字符|
|[a-z0-9]|匹配括号中的字符其中的任意字母或数字|
|[^a-z0-9]|匹配括号中的字符其中的任意字母或数字|
|\d|匹配数字|
|\D|匹配非数字|
|\w|匹配字母和数字及_|
|\W|匹配非字母和数字及_|

##锚字符
|元字符/元符号|匹配情况|
|:------:|:-------:|
|^|行首匹配|
|$|行尾匹配|
|\A|只有匹配字符串开始处|
|\G|匹配当前搜素的开始位置|
## 重复字符
|元字符/元符号|匹配情况|
|:------:|:-------:|
|x?|匹配0个或者1个x|
|x*|匹配0个或者任意多个x|
|x+|匹配0个或者任意多个x|




##match
##search
##replace
##split
##常用正则表示式
```
<script>
    var ad = /[0-9]{6}/;
    var str = "110022";
    alert(ad.test(str));
</script>
```
