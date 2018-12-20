####try/except 
```
try:
    <监控异常>
except Exception[,reason]:
    <异常处理代码>
finally:
    <无论异常是否发生都执行>
```
try语句块中捕获异常 except处理异常
```
try:
    f = open("data.txt","r")
    for line in f:
        print(line)
except ValueError as reason:
        print("出错")
else:
    print('没有异常')
finally:
    f.close()
```
对应不知道错位具体类型的可以直接使用Exception
```
try:
    a = 2 / 0
except Exception as e:
    print("%s" % e)
```
####自定义异常
继承Exception
```
class CustomError(Exception):
    def __str__(self):
        return "this is self error"


def custom_error_foo():
    try:
        raise CustomError()
    except CustomError as e:
        print("exception info:",e)

custom_error_foo()
```
