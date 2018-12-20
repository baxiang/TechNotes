## time函数
time()获取当前的时间戳,localtime()格式化当前的时间戳，转换成time.struct_time类型的对象.gmtime将时间戳转换成UTC时区的struct_time
```
time.time()
time.localtime()
time.gmtime()
```
time.struct_time
|属性|描述|
|-|-|
|tm_year|年的四位数|
|tm_mon|月|
|tm_mday|日|
|tm_hour|小时|
|tm_min|分钟|
|tm_sec|秒|
|tm_wday|一周的第几天0是周一6是周日|
|tm_yday|一年的第几天|
|tm_isdst|夏时令|
####mktime 
接收struct_time对象作为参数，返回秒为单位的时间戳
```
t =(2018,7,16,0,21,57,0,197,0)
t = time.mktime(t)
print(time.strftime("%Y-%m-%d %H:%M:%S",time.localtime(t)))
```
