####byte.Buffer
####扩容策略
```
b.buf = b.buf[:length+need]

```
新容器的容量 =2* 原有容量 + 所需字节数
####内容泄露
内容泄露是指，使用Buffer值的一方通过某种非标准的（或者说非正式的)的方法，得到了本该得不到的内容
####性能测试
```
func BenchmarkStrBuilder(b *testing.B) {
	str := strings.Builder{}
	str.WriteString("test")
	for i := 0; i < b.N; i++ {
		str.String()
	}
}

func BenchmarkByteBuffer(b *testing.B) {
	byte := bytes.Buffer{}
	byte.WriteString("test")
	for i := 0; i < b.N; i++ {
		byte.String()
	}
}
```
执行go test -bench . 其结果明显表明 strings.Builder转成string的性能好
```
BenchmarkStrBuilder-4           2000000000               0.43 ns/op
BenchmarkByteBuffer-4           200000000                7.33 ns/op
```
