####重定向
1.重定向输出
是将命令的正确输出结果保存到指定的文件中，重定向输出符号是">"和“>>",分别用于覆盖和追加文件，若重定向的文件不存在，则会创建文件，若要保存原来文件内容，需要使用">>",是追加新内容到文件底部
```
# echo "hello world" > readme.txt
# cat readme.txt
hello world
# echo "hello java" >> readme.txt
# cat readme.txt
hello world
hello java
```
2 错误重定向
错误重定向是把执行命令中出现的错误保存到指定文件，而不是显示到屏幕上来，错误重定向使用”2>"操作符，
```
$ ls test.txt 2>error.log
$ cat error.log
ls: test.txt: No such file or directory
```
/dev/null空文件
可以将无关紧要的信息输出到空文件中
&>
当命令输出的结果可能既包含标准输出又包含错误输出信息时，可以使用“&>"操作符合将两类输出信息保存到同一个文件。
