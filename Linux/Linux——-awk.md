####命令格式
```
awk '条件1{动作1}条件2{动作2}...' 文件名
```
####内置参数
$0:表示整个当前行
$1:每行第一个字段
$2: 每行第二个字段
NR 每行的记录号
NF 字段数量变量
FILENAME  正在处理的文件名
####分隔符
默认分隔符是空格 -F 指定分隔符
```
awk -F ':' '{print $1}' /etc/passwd
awk -F ':' '{print "Line: " NR, "Col: " NF,"User: " $1}' /etc/passwd
awk -F ':' '{printf("Line:%s Col:%s User:%s\n",NR,NF,$1)}' /etc/passwd
awk -F ':' '{if ($3>100) print "User:" $1}' /etc/passwd
```
####逻辑判断
~,!~: 匹配正则表达式
```
awk -F ':' '$1~/^my.*/{print $1}' /etc/passwd
```
==,!=,<,>判断逻辑表达式
```
awk -F ':' '$3>100{print $1,$3}' /etc/passwd 
```
####BEGIN&END
