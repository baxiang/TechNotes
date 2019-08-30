####net/http
一个简单的网络请求
``` golong
func helloWorld(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("hello world"))
}

func main() {
	http.HandleFunc("/hello", helloWorld)
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		log.Fatal(err)
	}
}
```
请求http://localhost:8080/hello会打印hello world

####go web的请求流程
![图片来源astaxie的build-web-application-with-golang
](https://upload-images.jianshu.io/upload_images/143845-8021cc71deaf0dc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
go10.1的源码
```
func (srv *Server) Serve(l net.Listener) error {
	defer l.Close()
	if fn := testHookServerServe; fn != nil {
		fn(srv, l)
	}
	var tempDelay time.Duration // how long to sleep on accept failure

	if err := srv.setupHTTP2_Serve(); err != nil {
		return err
	}

	srv.trackListener(l, true)
	defer srv.trackListener(l, false)

	baseCtx := context.Background() // base is always background, per Issue 16220
	ctx := context.WithValue(baseCtx, ServerContextKey, srv)
	for {
		rw, e := l.Accept()
		if e != nil {
			select {
			case <-srv.getDoneChan():
				return ErrServerClosed
			default:
			}
			if ne, ok := e.(net.Error); ok && ne.Temporary() {
				if tempDelay == 0 {
					tempDelay = 5 * time.Millisecond
				} else {
					tempDelay *= 2
				}
				if max := 1 * time.Second; tempDelay > max {
					tempDelay = max
				}
				srv.logf("http: Accept error: %v; retrying in %v", e, tempDelay)
				time.Sleep(tempDelay)
				continue
			}
			return e
		}
		tempDelay = 0
		c := srv.newConn(rw)
		c.setState(c.rwc, StateNew) // before Serve can return
		go c.serve(ctx)
	}
}
```
go c.serve()这里我们可以看到客户端的每次请求都会创建一个Conn，这个Conn里面保存了该次请求的信息，然后再传递到对应的handler，该handler中便可以读取到相应的header信息，这样保证了每个请求的独立性。

####路由器结构
```
type ServeMux struct {
	mu    sync.RWMutex
	m     map[string]muxEntry
	es    []muxEntry // slice of entries sorted from longest to shortest.
	hosts bool       // whether any patterns contain hostnames
}

type muxEntry struct {
	h       Handler
	pattern string
}
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)  // 路由实现器
}

```
Handler是一个接口，但是前一小节中的sayhelloName函数并没有实现ServeHTTP这个接口，为什么能添加呢？原来在http包里面还定义了一个类型HandlerFunc,我们定义的函数sayhelloName就是这个HandlerFunc调用之后的结果，这个类型默认就实现了ServeHTTP这个接口，即我们调用了HandlerFunc(f),强制类型转换f成为HandlerFunc类型，这样f就拥有了ServeHTTP方法。

####GET请求
```
import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func main() {
	resp, err := http.Get("http://httpbin.org/ip")
	if err != nil {
		fmt.Errorf("%s", err.Error())
		return
	}
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Errorf("%s", err.Error())
		return
	}
	fmt.Println(string(body))
}
```
####自定义request请求
```
func main() {
	query := map[string]string{"location": "北京",
		"ak": "VAuehGLIw7lW6ovwpnKboM3I", "output": "json"}
	u, _ := url.Parse("http://api.map.baidu.com/telematics/v3/weather")
	q := u.Query()
	for k, v := range query {
		q.Set(k, v)
	}
	u.RawQuery = q.Encode()
	req, err := http.NewRequest("GET", u.String(), nil)
	if err != nil {
		fmt.Errorf("%s", err.Error())
		return
	}
	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		fmt.Errorf("%s", err.Error())
		return
	}
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Errorf("%s", err.Error())
		return
	}
	fmt.Println(string(body))
}
```
####Post请求
```
func main() {
	url := "http://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule"
	payload := strings.NewReader("doctype=json&i=好好学习 天天向上")
	req, err := http.NewRequest("POST", url, payload)
	if err != nil {
		fmt.Errorf("%s", err.Error())
		return
	}
	req.Header.Add("content-type", "application/x-www-form-urlencoded")

	res, err := http.DefaultClient.Do(req)
	if err != nil {
		fmt.Errorf("%s", err.Error())
		return
	}
	defer res.Body.Close()
	body, err := ioutil.ReadAll(res.Body)
	if err != nil {
		fmt.Errorf("%s", err.Error())
		return
	}
	fmt.Println(string(body))
}

```
#### fasthttp
![image.png](https://upload-images.jianshu.io/upload_images/143845-e4d1f1a73e41dcc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

http Get请求
```
import (
	"fmt"
	"github.com/valyala/fasthttp"
	"log"
)

func main() {
	c := fasthttp.Client{}
	statusCode, body, err := c.Get(nil, "http://httpbin.org/ip")
	if statusCode == fasthttp.StatusOK {
		fmt.Println(string(body))
	} else {
		log.Fatalf("%d:%s", statusCode, err.Error())
	}
}
```
