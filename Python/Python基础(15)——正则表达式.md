####re.match() 
re.match(正则表达式,要匹配的字符串)，匹配出以字符串的起始位置开始匹配正则表达式,如果匹配，返回匹配对象（Match Object），否则返回None（注意不是空字符串""）。
``` python
import re
print(re.match('hello','hello world'))
print(re.match('world','hello world'))
# <_sre.SRE_Match object; span=(0, 5), match='hello'>
#None
```
####单字符匹配
|字符|说明|
|--|---|
|[]|匹配中括号里任意一个字符|
|[^]|除了中括号里的字符意外|
|\s|任意空格|
|\S|非空格|
|\d|匹配数字0-9|
|\D|匹配非数字，表示除了数字之外的字符|
|\w|任意字符[A-Za-z0-9]|
|\W|匹配非单词字符|
点号匹配
```
import re

result = re.match("A.C", "ABC")
print(result.group())
result = re.match("A.C", "A1C")
print(result.group())

```
中括号匹配
```
import re

result = re.match("A[B1D]C", "ABC")
print(result.group())
result = re.match("A[BCD]C", "AGC")
if result:
    print(result.group())
```
竖线匹配
```
text = "123test"
regexStr = "(123|txt)"
matchOjc = re.match(regexStr,text)
if matchOjc:
   print(matchOjc.group(1))
```
####开头结尾匹配
|字符|说明|
|--|---|
|^|以箭头后面的字符开头|
|$|以美元符号前面的字符结尾|
```
text = "test123"
regexStr = "^t.*3$"
if re.match(regexStr,text):
   print("match")
```
####多个字符匹配
|字符|说明|
|--|---|
|*|匹配前一个字符出现0次或者无限次，即可有可无|
|+|匹配前面的字符出现出现1次或者无限次，即至少有1次|
|?|匹配前一个字符出现1次或者0次，即要么有1次，要么没有,同时使贪婪变成非贪婪模式|
|{m}|匹配前一个字符出现m次|
|{m,n}|匹配前一个字符出现从m到n次|

```
import re

regStr = '[A-Za-z_]+[\w]*'
result = re.match(regStr, "test_1")
if result:
    print(result.group())
result = re.match(regStr, "1_test")
if result:
    print(result.group())
```
####匹配分组
|字符	|功能|
|---|----|
|\||匹配左右任意一个表达式|
|(ab)|将括号中字符作为一个分组|
|\num|引用第num分组匹配到的字符串|
|(?P<name>)	|分组起别名|
|(?P=name)	|引用别名为name分组匹配到的字符串|
```
import re

regStr = '[\w]{4,20}@(163|qq|gmail)\.com'
result = re.match(regStr, "test@qq.com")
if result:
    print(result.group())
```
'r'是防止字符转义的 如果路径中出现'\t'的话 不加r的话\t就会被转义 而加了'r'之后'\t'就能保留原有的样子。
```
import re

regStr = r'<(\w*)><(\w*)>.*</\2></\1>'
result = re.match(regStr, "<html><head>test</head></html>")
if result:
    print(result.group())
result = re.match(regStr, "<html><head>test</body></html>")
if result:
    print(result.group())
```
分组别名
```
import re

regStr = r'<(?P<label1>\w*)><(?P<label2>\w*)>.*</(?P=label2)></(?P=label1)>'
result = re.match(regStr, "<html><head>test</head></html>")
if result:
    print(result.group())
result = re.match(regStr, "<html><head>test</body></html>")
if result:
    print(result.group())

```
#### 匹配子表达式
使用小括号()将想要提取的内容括起来。
```
import re
content = 'hello 12345 python'
result = re.match('^h\w{4}\s(\d+)\s\w+',content)
print(result.group())
print(result.group(1))
print(result.span())
```
####贪婪匹配
点(.)可以匹配任意字符（除去换行符）星号（*）代表匹配前面字符的无限次。点星(.*)组合可以匹配任意字符，但是点星(.*)会匹配尽可能多的字符，被认为是贪婪匹配.贪婪匹配表达式^h.*(\d+)\s\w+造成group(1)只会得到数字7，因为点星(.*)会尽可能的取匹配字符，就把1234也吞噬了，只留下数字5了
```
import re
content = 'hello 12345 python'
result = re.match('^h.*(\d+)\s\w+',content)
print(result.group())
print(result.group(1))
print(result.span())
# hello 12345 python
# 5
# (0, 18)
```
####非贪婪匹配
非贪婪匹配的模式在点星后面加一个问号？即点星问(.*?)是非贪婪匹配，尽可能的少匹配字符
```
import re
content = 'hello 12345 python'
result = re.match('^h.*?(\d+)\s\w+',content)
print(result.group())
print(result.group(1))
print(result.span())
```
#### 匹配修饰符
|修饰符|说明|
|--|--|
|re.I|对大小写不敏感|
|re. L|做本地化识别匹配 |
|re. M|多行匹配，影响^和$ |
|re. S|匹配包括换行在内的所有字符 |
```
import re
content = '''hello 12345
python'''
result = re.match('^h.*?(\d+).*?n$',content,re.S)
print(result.group())
print(result.group(1))
print(result.span())
```

#### Search
匹配整个字符串，直到找到一个匹配的对象，匹配结束没有找到匹配对象才放回None
```
import re

result = re.search('\d+', "查找数字：1245")
if result:
    print(result.group())

```
####findall 
匹配所有符合规律的内容，返回包含结果的列表
```
import re

rList = re.findall('\d+', "查找数字：1245注册时11112中1")
for r in rList:
    print(r)
#1245
#11112
#1
```
####sub
re.sub(pattern, repl, string, count=0, flags=0)
使用repl替换string中每一个匹配的子串后返回替换后的字符串。 
当repl是一个字符串时，可以使用\id或\g<id>、\g<name>引用分组，但不能使用编号0。 
当repl是一个方法时，这个方法应当只接受一个参数（Match对象），并返回一个字符串用于替换（返回的字符串中不能再引用分组）。 
count用于指定最多替换次数，不指定时全部替换。
```
import re
content = 'hello12345python'
result = re.sub('\d',"",content)
print(result)
#hellopython
```

```
import re


def func(matchObj):
    if matchObj:
        return "python"

print(re.sub(r"\d+", func, 'hello 123'))
```
####split
根据匹配进行切割字符串，并返回一个列表
```
import re

rList = re.split(r':| ', "查找数字:1245注册时 11112中1")
for r in rList:
    print(r)
```
