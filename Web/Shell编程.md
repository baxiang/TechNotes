## 变量
####定义
```
variable=value
variable='value'
variable="value"
```
####变量命令规则
变量名由数字、字母、下划线组成；必须以字母或者下划线开头；不能使用 Shell 里的关键字（通过 help 命令可以查看保留关键字）。
####使用变量
使用一个定义过的变量，只要在变量名前面加美元符号$即可
####单引号和双引号的区别
```
#!/usr/bin/env bash

name="test_name"
name_single='我的名字：${name}'
name_double="我的名字：${name}"
echo $name_single
echo $name_double
#我的名字：${name}
#我的名字：test_name
```
#### 获取命令的结果赋值给变量
一种是把命令用反引号包围起来，另外一种是把命令用$()包围起来。
```
variable=`command`
variable=$(command)
```
```
#!/usr/bin/env bash

path=`pwd`
echo $path
list=$(ls)
echo $list
```
#### 只读变量
readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变。
```
#!/usr/bin/env bash

c="test only"
readonly c
c="update content"
```
#### 删除变量
```
#!/usr/bin/env bash

c="test content"
unset c
echo $c
```
## 字符串
```
#!/usr/bin/env bash

str_single='test  string1'
str_double="test string2 ${str_single}"
str_double2="\"$str_single\""
echo ${str_single}
echo ${str_double}
echo ${str_double2}
```
#### 获取字符串长度
```
str="12345"
echo ${#str}
```
#### 截取子字符串
```
str="12345"
echo ${str:1:3}
```
## 数组

####关系运算符
关系运算符只支持数字
-eq(Equal)相等返回true
-gt(Greater Than) 前边大返回true -ge 大于等于
-lt(Less than) 前边小返回true -le 小于等于
#### 字符串
-z 字符串长度为0 返回true
-n 字符串长度不为0 返回true
判断相等与不等 可以用==和!=
####文件
-f 判断文件是否为普通文件 是返回true
-x 判断文件是否可执行 是返回true
-d 判断文件是否为目录 是返回true
-w 判断文件是否可写 是返回true
-r 判断文件是否可读 是返回true
```
if [  ! -d ./test/ ]
then
    mkdir ./test/ #不存在就创建
else
   rm -rf ./test  #存在的就删除
fi
```
##流程控制
```
if [ expression ]
then
   Statement(s) to be executed if expression is true
fi
```
## 循环
#### for
```
for 变量 in 列表
do
    command1
    command2
    ...
    commandN
done
```
获取当前目录下所有的文件
```
for FILE in $(ls)
do
  echo $FILE
done
```
输出字符串
```
#!/usr/bin/env bash

for str in 'string content'
do
    echo $str
done
```
#### while循环
```
while command
do
   Statement(s) to be executed if command is true
done
```
