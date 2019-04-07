####socket
![图片.png](https://upload-images.jianshu.io/upload_images/143845-0dfc2677e4612139.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
客户端代码
```
package main

import (
	"bufio"
	"fmt"
	"io"
	"net"
	"time"
)

func main() {
	var tcpAddr *net.TCPAddr
	tcpAddr,_ = net.ResolveTCPAddr("tcp","127.0.0.1:9999")

	conn,err := net.DialTCP("tcp",nil,tcpAddr)

	if err!=nil {
		fmt.Println("Client connect error ! " + err.Error())
		return
	}else {
		fmt.Println(conn.LocalAddr().String() + "客户端连接成功!")
	}

	defer conn.Close()
	// 向服务器发送数据
	reader := bufio.NewReader(conn)
	conn.Write([]byte("hi server \n"))
	for {
		// 接受服务器返回数据
		msg, err := reader.ReadString('\n')
		fmt.Println(msg)
		if err != nil || err == io.EOF {
			fmt.Println(err)
			break
		}
		time.Sleep(time.Second * 2)

		b := []byte(fmt.Sprintf("client time:%s\n",time.Now().String()))
		_, err = conn.Write(b)

		if err != nil {
			fmt.Println(err)
			break
		}
	}
}
```
服务器代码
```
package main

import (
	"bufio"
	"fmt"
	"io"
	"net"
	"time"
)

//tcp server 

func main() {
	//定义一个tcp断点
	var tcpAddr *net.TCPAddr
	//通过ResolveTCPAddr实例一个具体的tcp断点
	tcpAddr,_ = net.ResolveTCPAddr("tcp",":9999")
	//打开一个tcp断点监听
	tcpListener,err:= net.ListenTCP("tcp",tcpAddr)
	if err !=nil{
		panic(err)
	}
	defer tcpListener.Close()
	fmt.Println("server start success")
	//循环接收客户端的连接，创建一个协程具体去处理连接
	for{
		tcpConn,err := tcpListener.AcceptTCP()
		if err!=nil {
			fmt.Println(err)
			continue
		}
		fmt.Println(tcpConn.RemoteAddr().String()+"connect success")
		go tcpPipe(tcpConn)
	}
}

//具体处理连接过程方法
func tcpPipe(conn *net.TCPConn){
	//tcp连接的地址
	ipStr := conn.RemoteAddr().String()

	defer func() {
		fmt.Println(" Disconnected : " + ipStr)
		conn.Close()
	}()
	//获取一个连接的reader读取流
	reader := bufio.NewReader(conn)
	for {
		message,_,err := reader.ReadLine()
		if err!=nil || err == io.EOF {
			break
		}
		fmt.Println(string(message))

		time.Sleep(time.Second*3)
		
		b := []byte(fmt.Sprintf("server time:%s\n",time.Now().String()))

		conn.Write(b)
		
	}
}
```
