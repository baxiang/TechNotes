####Reader接口
```
type Reader interface {
    Read(p []byte) (n int, err error)
}
```
```
io.Reader
io.ByteReader
io.RuneReader
io.ByteScanner
io.RuneScanner
io.WriterTo
```


####Writer 接口
```
io.Writer； 
io.ByteWriter；
io.stringWriter
io.ReaderFrom；
```
```
type Writer interface {
    Write(p []byte) (n int, err error)
}
```
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
