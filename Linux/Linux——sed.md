Sed是文本处理工具，根据指定条件对数据进行添加，替换，删除等操作。
-a 追加，在当前行后添加一行或者多行
-c 行替换 用c后面的字符串替换原数据行
-i 插入，在当前行插入一行或者多行
-d 删除 删除指定行
-p 打印 输出指定的行
-s 字符串替换 用一个字符串替换另外一个字符串，格式为"行范围s/旧字符串/新字符串/g",这个和vim中的替换格式类似。
####p打印命令
```
sed -n "p" passwd
```
####定位行
```
# sed -n "10p" passwd
operator:x:11:0:operator:/root:/sbin/nologin
# sed -n "/root/p" passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
```
行间隔
```
# cat passwd|sed -n '10,20p'
```
####a新增行
```
# nl passwd|sed '5a--------'
```
####i插入行
```
nl passwd|sed '5i--------'
```
####c 替换
```
nl passwd|sed '5c replace current line'
```
####d 删除
```
sed '1d' passwd #删除第一行数据
sed '2,4d' passwd #删除2~4行的数据
# nl passwd|sed '/mysql/ d' #删除匹配mysql数据
``` 
####$a末尾追加
```
sed '$a \\t int a = 1 \n\t System.out.Println(a)' test.sh
```
####-s 替换命令
