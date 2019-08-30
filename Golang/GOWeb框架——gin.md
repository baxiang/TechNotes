安装方式
```
go get -u github.com/gin-gonic/gin
```
创建web服务请求，使用gin的Default方法创建一个路由handler。然后通过HTTP方法绑定路由规则和路由函数。不同于net/http库的路由函数，gin进行了封装，把request和response都封装到gin.Context的上下文环境。
```
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
	"time"
)

func main() {
	router := gin.Default()
	router.GET("/", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"method": c.Request.Method,
			"time":   time.Now().Format("2006-01-02:15:04:05")})
	})
	router.Run(":8000")
}
```
#####RestAPI
```
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	router := gin.Default()
	c := func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{"method": c.Request.Method})
	}
	router.GET("/get", c)
	router.POST("/post", c)
	router.PUT("/put", c)
	router.DELETE("/delete", c)
	router.PATCH("/patch", c)
	router.HEAD("/head", c)
	router.OPTIONS("/options", c)
	router.Run(":8000")
}
```
