##连接
```
package main
import (
    "fmt"
    "github.com/garyburd/redigo/redis"
)
func main() {
    c, err := redis.Dial("tcp", "localhost:6379")
    if err != nil {
        fmt.Println("Connect to redis error", err)
        return
    }
    defer c.Close()
```

```
// 写入值永不过期
_, err = c.Do("SET", "username", "nick")
if err != nil {
    fmt.Println("redis set failed:", err)
}
username, err := redis.String(c.Do("GET", "username"))
if err != nil {
    fmt.Println("redis get failed:", err)
} else {
    fmt.Printf("Got username %v \n", username)
}
```
SET命令还支持附加参数

EX seconds -- 指定过期时间，单位为秒.
PX milliseconds -- 指定过期时间，单位为毫秒.
NX -- 仅当键值不存在时设置键值.
XX -- 仅当键值存在时设置键值. 
设置键值过期时间为10s
// 写入值10S后过期
_, err = c.Do("SET", "password", "123456", "EX", "10")
if err != nil {
    fmt.Println("redis set failed:", err)
}
time.Sleep(11 * time.Second)
password, err := redis.String(c.Do("GET", "password"))
if err != nil {
    fmt.Println("redis get failed:", err)
} else {
    fmt.Printf("Got password %v \n", password)
}
输出 
redis get failed: redigo: nil returned 
批量写入读取

MGET key [key ...]
MSET key value [key value ...]
批量写入读取对象(Hashtable)

HMSET key field value [field value ...]
HMGET key field [field ...]
检测值是否存在

EXISTS key
删除

DEL key [key ...]
设置过期时间

EXPIRE key seconds
