####拉模式与推送模式
拉模式的缺点 数据更新频率低，则大多数的请求是无效的
在线用户数量多，则服务端的查询负载高。
定时查询拉取，无法满足时效性要求
推送模式 尽在数据更新才推送，需要维护大量的在线长连接，数据更新后立即推送。
####WebSocket推送
浏览器支持的socket编程，轻松维护服务端长连接，基于TCP可靠传输之上的协议，无需开发者关心通讯细节。提供了高度抽象的编程接口，业务开发成本低。
####websocket协议
![来源于慕课网](https://upload-images.jianshu.io/upload_images/143845-f84d1c9c3ed8720c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
协议升级后，继续复用HTTP的底层socket完成后续操作
message底层被切分成多个frame 帧传输。
编程是只需要操作message不需要关心frame
框架底层完成TCP网络I/O,WebSocker协议解析，开发者不需要关心。
```
package main

import "net/http"

func wsHandle(writer http.ResponseWriter, request *http.Request) {
      writer.Write([]byte("hello world"))
}

func main() {
	http.HandleFunc("/ws", wsHandle)
	http.ListenAndServe(":8888",nil)
	
}
```
服务请求参数
```
Request URL: http://localhost:8888/ws
Request Method: GET
Status Code: 200 OK
Remote Address: [::1]:8888
```
websocket的服务器端代码
```
package main

import (
	"fmt"
	"github.com/gorilla/websocket"
	"net/http"
	"time"
)

func wsHandle(writer http.ResponseWriter, request *http.Request) {
	upgrader:= websocket.Upgrader{CheckOrigin: func(r *http.Request) bool {
		  return true
	  }}
 	con,err := upgrader.Upgrade(writer,request,nil)
 	defer con.Close()
	if err!=nil {
		writer.Write([]byte(err.Error()))
	}
	for{
		_, p, err := con.ReadMessage()
		if err!= nil {
			writer.Write([]byte(err.Error()))
			break
		}
		fmt.Println("client message "+string(p))
		con.WriteMessage(websocket.TextMessage,[]byte(time.Now().String()))
	}
}

func main() {
	http.HandleFunc("/ws", wsHandle)
	http.ListenAndServe(":8888",nil)
	
}
```

![image.png](https://upload-images.jianshu.io/upload_images/143845-29daadb76b64af9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
websocket 客户端代码
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <script>
        window.addEventListener("load", function(evt) {
            var output = document.getElementById("output");
            var input = document.getElementById("input");
            var ws;
            var print = function(message) {
                var d = document.createElement("div");
                d.innerHTML = message;
                output.appendChild(d);
            };
            document.getElementById("open").onclick = function(evt) {
                if (ws) {
                    return false;
                }
                ws = new WebSocket("ws://localhost:8888/ws");
                ws.onopen = function(evt) {
                    print("连接websocket");
                }
                ws.onclose = function(evt) {
                    print("CLOSE");
                    ws = null;
                }
                ws.onmessage = function(evt) {
                    print("收到消息: " + evt.data);
                }
                ws.onerror = function(evt) {
                    print("ERROR: " + evt.data);
                }
                return false;
            };
            document.getElementById("send").onclick = function(evt) {
                if (!ws) {
                    return false;
                }
                print("发送消息: " + input.value);
                ws.send(input.value);
                return false;
            };
            document.getElementById("close").onclick = function(evt) {
                if (!ws) {
                    return false;
                }
                ws.close();
                return false;
            };
        });
    </script>
</head>
<body>
<table>
    <tr><td valign="top" width="50%">
            <p>点击启动按钮创建websocket连接<br/>
               点击发送按钮可以发送任意消息给服务器<br/>
               点击关闭按钮断开websocket连接
            </p>
            <form>
                <button id="open">启动</button>
                <button id="close">关闭</button>
                <input id="input" type="text" value="Hello golang!">
                <button id="send">发送</button>
            </form>
        </td><td valign="top" width="50%">
            <div id="output"></div>
        </td></tr></table>
</body>
</html>
```
运行client.html,效果如下
![](https://upload-images.jianshu.io/upload_images/143845-2ae74638dca37a36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
数据抓包
![image.png](https://upload-images.jianshu.io/upload_images/143845-5021084175e458b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
