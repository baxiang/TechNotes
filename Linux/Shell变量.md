定义变量的基本格式为"变量名=变量值",注意等号两边没有空格，在变量名称前面添加美元符号"$",可以引用一个变量的值，使用echo命令可以查看变量。
```
# version=6.0
# echo $version
6.0
```
**双引号**
双引号主要界定字符串的作用，当内容中出现空格的时候，在双引号范围内，使用“$"符号可以应用其他变量.
```
#people="人民"
# echo "中国$people"
中国人民
```
**反撇号（`）**
反撇号主要用于命令替换，允许将执行某个命令的屏幕输出结果赋值给·变量。反撇号包裹的的字符内容必须是可以执行的命令行。
```
$ ls -la `which ls`
-rwxr-xr-x  1 root  wheel  38704 11 30 15:39 /bin/ls
```
**read命令**
read提示用户输入信息，从而实现简单的交互过程，执行的时候从标注输入读入一行内容
```
$ read -p "请输入内容: " inputVal
请输入内容: test content
$ echo $inputVal
test content
```
**export**
export指定的变量成为全局变量
**变量运算**
只能进行简单的整数运算，基本格式如下，注意运算符和变量之间必须至少有一个空格,变量必须是整数，不能是字符串或者小数
```
 expr 变量1 运算符 变量2
# x=1
# y=2
# expr $x + $y
```
整数运算还可以使用`$(())`,注意是双层括号
```
# sum=$((1+2))
#echo $sum
#echo $((1+2+3))
```
环境变量
env查看当前工作环境下的环境变量，PATH变量用于设置可执行程序的默认搜索路径，Linux系统将在PATH变量指定的目录范围查找对应的可执行文件，如果找不到会提示“command not found",HOME 表示用户宿主的主目录
```
PATH=/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/baxiang/.local/bin:/home/baxiang/bin
PWD=/home/baxiang
LANG=zh_CN.UTF-8
SELINUX_LEVEL_REQUESTED=
HISTCONTROL=ignoredups
SHLVL=1
HOME=/home/baxiang
LOGNAME=baxiang
```
**位置变量**
位置变量也叫位置参数
\$0对应的是当前Shell脚本程序的名称。
\$#对应的是总共有几个参数。
\$*对应的是所有位置的参数值。
\$?对应的是显示上一次命令的执行返回值。
\$1、\$2、\$3……则分别对应着第N个位置的参数值。
```
echo "脚本名称$0"
echo "总共 $#个参数，分别是$*"
echo "第一个参数是$1,第五个参数$5"
```
执行脚本
```
# sh test.sh one two three four five six
脚本名称test.sh
总共 6个参数，分别是one two three four five six
第一个参数是one,第五个参数five
```
