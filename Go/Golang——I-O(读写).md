## 终端读写
操作终端相关文件句柄常量
####缓冲区读写
## 文件读写
os.File封装所有文件相关的读写
```
package main

import (
	"os"
	"fmt"
	"bufio"
	"io"
)

func main(){
	file,err :=os.Open("shellDemo.sh")
	defer file.Close()
	if err!= nil {
		fmt.Println(err)
		return
	}
	reader := bufio.NewReader(file)
	for {
		data,prefix,err := reader.ReadLine()
		if err ==io.EOF {
			break
		}
		fmt.Printf("data%s-prefix:%v\n",string(data),prefix)
	}
}

```
