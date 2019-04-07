
## 目录操作
创建名称为name的目录，权限设置是perm，例如0777
```
func Mkdir(name string, perm FileMode) error
```
	
根据path创建多级子目录，例如astaxie/test1/test2。
```	
 func MkdirAll(path string, perm FileMode) error
```
删除名称为name的目录，当目录下有文件或者其他目录时会出错
```
func Remove(name string) error
```
根据path删除多级子目录，如果path是单个名称，那么该目录下的子目录全部删除。	
```
func RemoveAll(path string) error
```

eg：
```Go

package main

import (
	"fmt"
	"os"
)

func main() {
	os.Mkdir("test", 0777)
	os.MkdirAll("test/test1/test2", 0777)
	err := os.Remove("test")
	if err != nil {
		fmt.Println(err)
	}
	os.RemoveAll("test")
}

```

## 文件操作

### 建立文件
根据提供的文件名创建新的文件，返回一个文件对象，默认权限是0666的文件，返回的文件对象是可读写的。
```
 func Create(name string) (file *File, err Error)
```
根据文件描述符创建相应的文件，返回一个文件对象
```
 func NewFile(fd uintptr, name string) *File
```
###打开文件

该方法打开一个名称为name的文件，但是是只读方式，内部实现其实调用了OpenFile。
```
- func Open(name string) (file *File, err Error)
```	
打开名称为name的文件，flag是打开的方式，只读、读写等，perm是权限	
```
 func OpenFile(name string, flag int, perm uint32) (file *File, err Error)	
```
### 写文件
写入byte类型的信息到文件
```
 func (file *File) Write(b []byte) (n int, err Error)
```
在指定位置开始写入byte类型的信息
```
 func (file *File) WriteAt(b []byte, off int64) (n int, err Error)

```
写入string信息到文件
```
 func (file *File) WriteString(s string) (ret int, err Error)
```	

### 读文件
读取数据到b中
```
 func (file *File) Read(b []byte) (n int, err Error)
```
从off开始读取数据到b中	
```
func (file *File) ReadAt(b []byte, off int64) (n int, err Error)
```
eg:
```Go

package main

import (
	"fmt"
	"os"
)

func main() {
    fileName := "testFile"
    currFile,err:=os.Create(fileName)
    if err!= nil {
        fmt.Println(err,currFile);
    }
    currFile.WriteString("hello world\n")
    currFile.Write([]byte("hello go\n"))
    currFile,err = os.Open(fileName);
    defer currFile.Close()
    buf := make([]byte,1024)
    for  {
       n,_:=currFile.Read(buf)
        if 0==n {
          break
        }
        os.Stdout.Write(buf[:n])
    }
}

```
### 删文件
Go语言里面删除文件和删除文件夹是同一个函数,	调用该函数就可以删除文件名为name的文件
```
func Remove(name string) Error
```

```
// Discard 是一个 io.Writer 接口，调用它的 Write 方法将不做任何事情
// 并且始终成功返回。
var Discard io.Writer = devNull(0)

// ReadAll 读取 r 中的所有数据，返回读取的数据和遇到的错误。
// 如果读取成功，则 err 返回 nil，而不是 EOF，因为 ReadAll 定义为读取
// 所有数据，所以不会把 EOF 当做错误处理。
func ReadAll(r io.Reader) ([]byte, error)

// ReadFile 读取文件中的所有数据，返回读取的数据和遇到的错误。
// 如果读取成功，则 err 返回 nil，而不是 EOF
func ReadFile(filename string) ([]byte, error)

// WriteFile 向文件中写入数据，写入前会清空文件。
// 如果文件不存在，则会以指定的权限创建该文件。
// 返回遇到的错误。
func WriteFile(filename string, data []byte, perm os.FileMode) error

// ReadDir 读取指定目录中的所有目录和文件（不包括子目录）。
// 返回读取到的文件信息列表和遇到的错误，列表是经过排序的。
func ReadDir(dirname string) ([]os.FileInfo, error)

// NopCloser 将 r 包装为一个 ReadCloser 类型，但 Close 方法不做任何事情。
func NopCloser(r io.Reader) io.ReadCloser

// TempFile 在 dir 目录中创建一个以 prefix 为前缀的临时文件，并将其以读
// 写模式打开。返回创建的文件对象和遇到的错误。
// 如果 dir 为空，则在默认的临时目录中创建文件（参见 os.TempDir），多次
// 调用会创建不同的临时文件，调用者可以通过 f.Name() 获取文件的完整路径。
// 调用本函数所创建的临时文件，应该由调用者自己删除。
func TempFile(dir, prefix string) (f *os.File, err error)

// TempDir 功能同 TempFile，只不过创建的是目录，返回目录的完整路径。
func TempDir(dir, prefix string) (name string, err error)
```

