根据模式搜索文本，并将符合模式的文本进行显示出来。
用法: 
```
grep [选项]... PATTERN [FILE]...
```
在每个 FILE 或是标准输入中查找 PATTERN。
默认的 PATTERN 是一个基本正则表达式(缩写为 BRE)。
例如: grep -i 'hello world' menu.h main.c

正则表达式选择与解释:
```
  -E, --extended-regexp     PATTERN 是一个可扩展的正则表达式(缩写为 ERE)
  -F, --fixed-strings       PATTERN 是一组由断行符分隔的定长字符串。
  -G, --basic-regexp        PATTERN 是一个基本正则表达式(缩写为 BRE)
  -P, --perl-regexp         PATTERN 是一个 Perl 正则表达式
  -e, --regexp=PATTERN      用 PATTERN 来进行匹配操作
  -f, --file=FILE           从 FILE 中取得 PATTERN
  -i, --ignore-case         忽略大小写
  -w, --word-regexp         强制 PATTERN 仅完全匹配字词
  -x, --line-regexp         强制 PATTERN 仅完全匹配一行
  -z, --null-data           一个 0 字节的数据行，但不是空行
  -v  显示没有被模式匹配的行
   -o 只显示被模式匹配的字符串
```
查找用户包含root的
```
 $grep 'root' /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
```
####Regular Expression正则表达式
* :匹配其前面的字符任意次
？:匹配前面的子表达式0次或1次。
. :表示任意单个字符
.*：任意长度的任意字符
