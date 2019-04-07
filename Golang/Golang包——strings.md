
####包含关系
```
// 子串substr在s中，返回true
func Contains(s, substr string) bool
// chars中任何一个Unicode代码点在s中，返回true
func ContainsAny(s, chars string) bool
// Unicode代码点r在s中，返回true
func ContainsRune(s string, r rune) bool
```
####字符串转换
下面行为会出现数据越界，但是会返回最大值127
```
func TestStrConInt(t *testing.T) {
	n, err := strconv.ParseInt("128", 10, 8)
	if err != nil {
		t.Log(err)
	}
	t.Log(n)
}
```
####strings.Builder
优势
已存在的内容不可变，但是可以拼接新内容
减
####strings.Reader
Reader 类型
看到名字就能猜到，这是实现了 io 包中的接口。它实现了 
```
io.Reader（Read 方法）
io.ReaderAt（ReadAt 方法）
io.Seeker（Seek 方法）
io.WriterTo（WriteTo 方法）
io.ByteReader（ReadByte 方法）
io.ByteScanner（ReadByte 和 UnreadByte 方法）
io.RuneReader（ReadRune 方法）
io.RuneScanner（ReadRune 和 UnreadRune 方法）。
```

Reader 结构如下：
```
type Reader struct {
    s        string    // Reader 读取的数据来源
    i        int // current reading index（当前读的索引位置）
    prevRune int // index of previous rune; or < 0（前一个读取的 rune 索引位置）
}
```
可见 Reader 结构没有导出任何字段，而是提供一个实例化方法：
```
func NewReader(s string) *Reader
```
```
sr := strings.NewReader("0123456789A")
	b := make([]byte, 5)
	for {
		n, err := sr.Read(b)
		if err != nil {
			fmt.Println(err)
			break
		}
		fmt.Printf("%d %q\n", n, b[:n])
	}
```
打印结果是
```
5 "01234"
5 "56789"
1 "A"
EOF

```
