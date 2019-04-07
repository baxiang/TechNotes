bufio是“buffered I/O”的缩写
#####bufio.Reader
```
type Reader struct {
    buf          []byte        // 缓存
    rd           io.Reader    // 底层的io.Reader
    // r:从buf中读走的字节（偏移）；w:buf中填充内容的偏移；
    // w - r 是buf中可被读的长度（缓存数据的大小），也是Buffered()方法的返回值
    r, w         int
    err          error        // 读过程中遇到的错误
    lastByte     int        // 最后一次读到的字节（ReadByte/UnreadByte)
    lastRuneSize int        // 最后一次读到的Rune的大小(ReadRune/UnreadRune)
}
```
######初始化函数
```
//NewReader函数用来返回一个默认大小buffer的Reader对象(默认大小好像是4096) 等同于NewReaderSize(rd,4096)
func NewReader(rd io.Reader) *Reader

//该函数返回一个指定大小buffer(size最小为16)的Reader对象，如果 io.Reader参数已经是一个足够大的Reader，它将返回该Reader
func NewReaderSize(rd io.Reader, size int) *Reader

```
######读取函数
```
//该方法返回从当前buffer中能被读到的字节数
func (b *Reader) Buffered() int

//Discard方法跳过后续的 n 个字节的数据，返回跳过的字节数。如果0 <= n <= b.Buffered(),该方法将不会从io.Reader中成功读取数据。
func (b *Reader) Discard(n int) (discarded int, err error)

//Peekf方法返回缓存的一个切片，该切片只包含缓存中的前n个字节的数据
func (b *Reader) Peek(n int) ([]byte, error)

//把Reader缓存对象中的数据读入到[]byte类型的p中，并返回读取的字节数。读取成功，err将返回空值
func (b *Reader) Read(p []byte) (n int, err error)

//返回单个字节，如果没有数据返回err
func (b *Reader) ReadByte() (byte, error)

//该方法在b中读取delimz之前的所有数据，返回的切片是已读出的数据的引用，切片中的数据在下一次的读取操作之前是有效的。如果未找到delim，将返回查找结果并返回nil空值。因为缓存的数据可能被下一次的读写操作修改，因此一般使用ReadBytes或者ReadString，他们返回的都是数据拷贝
func (b *Reader) ReadSlice(delim byte) (line []byte, err error)

//功能同ReadSlice，返回数据的拷贝
func (b *Reader) ReadBytes(delim byte) ([]byte, error)

//功能同ReadBytes,返回字符串
func (b *Reader) ReadString(delim byte) (string, error)

//该方法是一个低水平的读取方式，一般建议使用ReadBytes('\n') 或 ReadString('\n')，或者使用一个 Scanner来代替。ReadLine 通过调用 ReadSlice 方法实现，返回的也是缓存的切片，用于读取一行数据，不包括行尾标记（\n 或 \r\n）
func (b *Reader) ReadLine() (line []byte, isPrefix bool, err error)

//读取单个UTF-8字符并返回一个rune和字节大小
func (b *Reader) ReadRune() (r rune, size int, err error)

```
按照行读取文件
```
f, _ := os.Open("demo.txt")
	reader := bufio.NewReader(f)
	for {
		line, isPrefix, err := reader.ReadLine()
		if err != nil {
			fmt.Println(err)
			break
		}
		fmt.Println(string(line), isPrefix)
	}
```
####bufio.Writer
写入缓冲流
```
type Writer struct {
	err error
	buf []byte
	n   int
	wr  io.Writer
}
```
```
//Writer是一个空的结构体，一般需要使用NewWriter或者NewWriterSize来初始化一个结构体对象

//NewWriterSize和NewWriter函数
//返回默认缓冲大小的Writer对象(默认是4096)
func NewWriter(w io.Writer) *Writer

//指定缓冲大小创建一个Writer对象
func NewWriterSize(w io.Writer, size int) *Writer
```
写入函数
```
//把p中的内容写入buffer,返回写入的字节数和错误信息。如果nn<len(p),返回错误信息中会包含为什么写入的数据比较短
func (b *Writer) Write(p []byte) (nn int, err error)
//将buffer中的数据写入 io.Writer
func (b *Writer) Flush() error

//以下三个方法可以直接写入到文件中
//写入单个字节
func (b *Writer) WriteByte(c byte) error
//写入单个Unicode指针返回写入字节数错误信息
func (b *Writer) WriteRune(r rune) (size int, err error)
//写入字符串并返回写入字节数和错误信息
func (b *Writer) WriteString(s string) (int, error)

```
