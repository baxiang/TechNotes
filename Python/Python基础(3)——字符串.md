####字符串语法
双引号或者单引号中的数据，就是字符串
```
str = "hellow world"
string = 'hellow python'
```
####字符串的输入输出
```
name = input("请输入姓名：")
print(name)
age = input("请输入年龄：")
print(age)
```
####字符串格式化
字符串格式化使用操作符百分号%实现
```
print("hello,%s" % "world")
```
浮点型数字指定精度
```
print("圆周率PI的值是%.2f" % 2.1415926)
```
字符串和元组
```
print("我的名字叫%s,我今年%d,我现在在%s" % ("bx", 25, "beijing"))
```
####字符串索引
```
str = "123456"   
print(str[2])    
print(str[-1])   
```
####字符串切片
切片的语法：[起始:结束:步长]，选取的区间属于左闭右开型，即从"起始"位开始，到"结束"位的前一位结束（不包含结束位本身)。
```
name = "hello world"
print(name[0:3])
```
#### 多行数据字符串表示
'''...'''的格式表示多行内容
```
str = """1
22
333
4444
"""
print(str)
```
####字符串编码和解码
```
s =u'你好'
e = s.encode('utf-8')
print(e)
d = e.decode('utf-8')
print(d)
#b'\xe4\xbd\xa0\xe5\xa5\xbd'
#你好
```

## 字符串常用方法
#### find
检测 str 是否包含在 mystr中，如果是返回开始的索引值，否则返回-1,index跟find()方法一样，只不过如果str不在 mystr中会报一个异常.
```
str = "123456"          
print(str.find("2"))    
print(str.index("3"))   
```
####count
返回 str在start和end之间 在 mystr里面出现的次数
```
str = "1234565"         
print(str.count("5"))   
```
#### startwith
字符串是否是以 prefix 开头, 是则返回 True，否则返回 False
```
str = "hello,python"            
print(str.startswith("hello"))  
True
```
#### endswith
字符串是否是以 suffix结束，如果是返回True,否则返回 False.
```                          
str = "hello,python"          
print(str.endswith("world"))  
```
####join
```
iterStr = "123"            
str = ","                  
print(str.join(iterStr)) 
1,2,3  
```
####isalpha()
字符串是否全部是字母
```str = "hello,Python"
print(str.isalpha())//False
str = "HelloPython"
print(str.isalpha())//True
```
####isdigit()
字符串是否全部是数字
```
str = "0123456789"
print(str.isalnum())
```
####capitalize
首字母变成大写,其余字母小写
```
str = "hello,Python"     
print(str.capitalize())  
Hello,python
```
####title
非字母隔开的部分,首字母大写,其余字母小写
```
str = "hello,python"  
print(str.title())    
Hello,Python
```
####lower
将字符串都转成小写
```
str = "Hello,Python"    
print(str.lower())      
hello,python
```
####upper
将字符串中所有的小写转成大写
```                   
str = "hello,python
print(str.upper()) 
HELLO,PYTHON
```
####swapcase 
将大写转成小写,小写转成大写
```
str = "hELLO,pYTHON"    
print(str.swapcase())   
Hello,Python
```
####replace
把 string 中的 str1 替换成 str2,如果 count 指定，则替换不超过 count 次.
```
str = "hello world"                     
print(str.replace("world", "python"))   
```
####split
以 sep 为分隔符切片 str，如果 maxsplit有指定值，则仅分隔 maxsplit 个子字符串
```
str = "hello,python"  
print(str.split(",")) 
```

