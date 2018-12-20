## Time 常用函数
```
//获取当前时间,返回Time类型
func Now() Time

Unix(sec int64, nsec int64) Time
//根据秒数和纳秒,返回Time类型
Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time
//设置年月日返回,Time类型
Since(t Time) Duration//返回与当前时间的时间差
func (t Time) UTC() Time
func (t Time) Unix() int64
func (t Time) UnixNano() int64
```
##常用的时间单位的换算
毫秒和纳秒是两的时间单位 1秒=1000毫秒 1毫秒=1000微秒 1微秒=1000纳秒
```
fmt.Println(time.Now().Unix())                       //获取当前时间戳 单位是秒
fmt.Println(time.Now().UnixNano())                   //获取当前时间抽 单位是纳秒
fmt.Println(time.Now().UnixNano() / 1e6)             //将纳秒转换为毫秒
fmt.Println(time.Now().UnixNano() / 1e9)             //将纳秒转换为秒
fmt.Println(time.Unix(time.Now().UnixNano()/1e9, 0)) //将毫秒转换为 time 类型            
```
##Time常用方法
```
//时间类型比较,是否在Time之后
After(u Time) bool 
//时间类型比较,是否在Time之前
Before(u Time) bool 
//比较两个时间是否相等
Equal(u Time) bool 
//判断时间是否为零值,如果sec和nsec两个属性都是0的话,则该时间类型为0
IsZero() bool 
//返回年月日,三个参数
Date() (year int, month Month, day int) 
//返回年份
Year() int 
//返回月份.是Month类型
Month() Month 
//返回多少号
Day() int 
//返回星期几,是Weekday类型
Weekday() Weekday 
//返回年份,和该填是在这年的第几周.
ISOWeek() (year, week int) 
//返回小时,分钟,秒
Clock() (hour, min, sec int) 
//返回小时
Hour() int 
//返回分钟
Minute() int 
//返回秒数
Second() int 
//返回纳秒
Nanosecond() int 
```
##timer 计时器
```
// 使用AfterFunc
time.AfterFunc(5 * time.Minute, func() {
    fmt.Printf("expired")
}
timer := time.NewTimer(5 * time.Minute)
<-timer.C
fmt.Printf("expired")
####time.Newtimer函数
```
初始化三小时30分钟的定时器
```
t := time.Newtimer(3*time.Hour + 30*time.Minute)
```
重置定时器
```
func (t *Timer) Reset(d Duration) bool
```
停止定时器,如果返回false，说明该定时器在之前已经到期或者已经被停止了,反之返回true。```
func (t *Timer) Stop() bool 
```
```
func main() {
	t := time.NewTimer(5 * time.Second)
	//当前时间
	now := time.Now()
	fmt.Printf("Now time : %v.\n", now)

	expireTime := <-t.C
	fmt.Printf("Expiration time: %v.\n", expireTime)

}
```
##ticker 断续器
