####Reader接口
```
type Reader interface {
    Read(p []byte) (n int, err error)
}
```
Reader 接口包装了基本的 Read 方法，用于输出自身的数据。Read 方法用于将对象的数据流读入到 p 中，返回读取的字节数和遇到的错误。
如果读到了数据（n > 0），则 err 应该返回 nil。如果数据被读空，没有数据可读（n == 0），则 err 应该返回 EOF。 如果遇到读取错误，则 err 应该返回相应的错误信息。
####Reader扩展接口
```
type ReaderFrom interface {
    ReadFrom(r Reader) (n int64, err error)
}
```
 ReaderFrom 接口包装了基本的 ReadFrom 方法，用于从 r 中读取数据存入自身。 直到遇到 EOF 或读取出错为止，返回读取的字节数和遇到的错误。
```
type ReaderAt interface {
    ReadAt(p []byte, off int64) (n int, err error)
}
```
```
type ByteReader interface {
    ReadByte() (c byte, err error)
}
```
ByteReader 接口包装了基本的 ReadByte 方法，用于从自身读出一个字节。返回读出的字节和遇到的错误。
```
type ByteScanner interface {
    ByteReader
    UnreadByte() error
}
```
ByteScanner 在 ByteReader 的基础上增加了一个 UnreadByte 方法，用于撤消最后一次的 ReadByte 操作，以便下次的 ReadByte 操作可以读出与前一次一样的数据。
UnreadByte 之前必须是 ReadByte 才能撤消成功，否则可能会返回一个错误信息（根据不同的需求，UnreadByte 也可能返回 nil，允许随意调用 UnreadByte，但只有最后一次的 ReadByte 可以被撤销，其它 UnreadByte 不执行任何操作）
```
type RuneReader interface {
    ReadRune() (r rune, size int, err error)
}
```
 RuneReader 接口包装了基本的 ReadRune 方法，用于从自身读取一个 UTF-8 编码的字符到 r 中。返回读取的字符、字符的编码长度和遇到的错误。
```
type RuneScanner interface {
    RuneReader
    UnreadRune() error
}
```
RuneScanner 在 RuneReader 的基础上增加了一个 UnreadRune 方法，用于撤消最后一次的 ReadRune 操作，以便下次的 ReadRune 操作可以读出与前一次一样的数据。
 UnreadRune 之前必须是 ReadRune 才能撤消成功，否则可能会返回一个错误信息（根据不同的需求，UnreadRune 也可能返回 nil，允许随意调用 UnreadRune，但只有最后一次的 ReadRune 可以被撤销，其它 UnreadRune 不执行任何操作）。
####自定义实现reader接口
```
package main

import (
	"io"
)

type UpperString struct {
	s string
	i int64
}

func NewUpperString(s string)*UpperString{
	return &UpperString{s,0}
}

func (upperStr *UpperString)Len()int{
	if upperStr.i >= int64(len(upperStr.s)) {
		return 0
	}
	return int(int64(len(upperStr.s)) - upperStr.i)
}

func (upperStr *UpperString) Read(b []byte) (n int, err error) {
	for ; n<len(b)&&int(upperStr.i)<len(upperStr.s); upperStr.i++ {
		c := upperStr.s[upperStr.i]
		if c >= 'a' && c <= 'z' {
			c -= 'a' - 'A'
		}
		b[n] = c
		n++
	}
	if n == 0 {
		return n, io.EOF
	}
	return n, nil
}

func main() {
    	str := NewUpperString("abcderghijkmnopq")
	b := make([]byte, str.Len())
	n, _ := str.Read(b)
	println(string(b[:n]))

}
```
####Writer 接口

```
type Writer interface {
    Write(p []byte) (n int, err error)
}
```
 Writer 接口包装了基本的 Write 方法，用于将数据存入自身。
 Write 方法用于将 p 中的数据写入到对象的数据流中，
 返回写入的字节数和遇到的错误。
如果 p 中的数据全部被写入，则 err 应该返回 nil。
如果 p 中的数据无法被全部写入，则 err 应该返回相应的错误信息。
#####Write扩展接口
```
type WriterTo interface {
    WriteTo(w Writer) (n int64, err error)
}
```
WriterTo 接口包装了基本的 WriteTo 方法，用于将自身的数据写入 w 中。直到数据全部写入完毕或遇到错误为止，返回写入的字节数和遇到的错误。
```
type WriterTo interface {
    WriteTo(w Writer) (n int64, err error)
}
```
WriterTo 接口包装了基本的 WriteTo 方法，用于将自身的数据写入 w 中。直到数据全部写入完毕或遇到错误为止，返回写入的字节数和遇到的错误。
```
type ByteWriter interface {
    WriteByte(c byte) error
}
```
 ByteWriter 接口包装了基本的 WriteByte 方法，用于将一个字节写入自身


####copy
```go
src := strings.NewReader("1234567890")
	dst := new(strings.Builder)
	written, err := io.CopyN(dst, src, 8)
	if err != nil {
		fmt.Printf("error: %v\n", err)
	} else {
		fmt.Printf("Written(%d): %q\n", written, dst.String())
	}
```
####os.file
```go
O_RDONLY	//syscall.O_RDONLY(0x00000)只读
O_WRONLY	//syscall.O_WRONLY(0x00001)只写
O_RDWR		//syscall.O_RDWR(0x00002)可读可写
O_CREATE	//syscall.O_CREAT(0x00040)创建文件
O_EXCL		//syscall.O_EXCL(0x00080)配合O_CREATE使用，在创建文件时如果该文件已存在，则提示错误；open xxx: The file exists.
O_TRUNC		//syscall.O_TRUNC(0x00200)清零
O_APPEND	//syscall.O_APPEND(0x00400)续写
O_SYNC		//syscall.O_SYNC(0x01000)同步IO，表示对文件的更改会强制同步到硬盘，而不是写入缓存区后由系统写入硬盘
```
####Closer
```
type Closer interface {
    Close() error
}
Closer 接口包装了基本的 Close 方法，用于关闭数据读写。
Close 一般用于关闭文件，关闭通道，关闭连接，关闭数据库等
```
####Seeker
```
type Seeker interface {
    Seek(offset int64, whence int) (ret int64, err error)
}
```
 Seeker 接口包装了基本的 Seek 方法，用于移动数据的读写指针。Seek 设置下一次读写操作的指针位置，每次的读写操作都是从指针位置开始的。
 whence 的含义是：
如果 whence 为 0：表示从数据的开头开始移动指针。
如果 whence 为 1：表示从数据的当前指针位置开始移动指针。
如果 whence 为 2：表示从数据的尾部开始移动指针。
offset 是指针移动的偏移量。
ret返回新指针位置
####接口组合
```
type ReadWriter interface {
    Reader
    Writer
}

type ReadSeeker interface {
    Reader
    Seeker
}

type WriteSeeker interface {
    Writer
    Seeker
}

type ReadWriteSeeker interface {
    Reader
    Writer
    Seeker
}

type ReadCloser interface {
    Reader
    Closer
}

type WriteCloser interface {
    Writer
    Closer
}

type ReadWriteCloser interface {
    Reader
    Writer
    Closer
}
```

####附录
|方法|说明|
|----|---|
|func OpenFile(name string, flag int, perm FileMode) (*File, error)	|通用文件打开函数，其中多个标志位可采取\|\|方式携带，perm为文件权限|
|func Open(name string) (*File, error)	|									以只读方式打开文件O_RDONLY|
|func Create(name string) (*File, error)	|							创建文件，且打开文件方式为可读、可写、trunc|
|func (f *File) Seek(offset int64, whence int) (ret int64, err error)	|返回offset，其中whence可指定offset是相对于哪里（0表示文件开头，1表示当前offset，2表示文件结尾），需要注意的是，对于以APPEND模式打开的文件，官方文档表示行为不确定。The behavior of Seek on a file opened with O_APPEND is not specified.|
|func (f *File) Read(b []byte) (n int, err error) ||
|func (f *File) ReadAt(b []byte, off int64) (n int, err error)||
|func (f *File) Write(b []byte) (n int, err error) ||
|func (f *File) WriteAt(b []byte, off int64) (n int, err error)|||
|func Rename(oldname string, newname string) (err error) |					//文件改名，若newname已存在则覆盖原有文件，即删除newname再将oldname改名；注意文件名可携带相对或绝对路径且更改前后文件夹路径应一致，否则会被拒绝操作；注意Windows下路径的“\”需要转义|
|func Chdir(dir string) error	|更改当前工作文件夹|
|func Mkdir(name string, perm FileMode) error|						创建文件夹
|func Chmod(name string, mode FileMode) error||
|func (f *File) Chmod(mode FileMode) error|||
