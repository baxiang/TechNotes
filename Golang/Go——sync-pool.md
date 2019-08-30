尝试从私有对象获取
私有对象不存在，尝试从当前Processor的共享池获取
如果当前Processor共享池是空的，那么就尝试去其他Processor的共享池获取
如果所有子池都是空的，最后就用指定的New 函数产生一个新的对象返回。
####放回
如果私有对象不存在则保存为私有对象
如果私有对象存在，放入当前Processor子池的共享池
####生命周期
GC会清除sync.pool缓存的对象
对象的缓存有效期为下一次GC之前
```
func TestSyncPool(t *testing.T) {
	pool := &sync.Pool{New: func() interface{} {
		return 2
	}}
	a, ok := pool.Get().(int)
	if ok {
		fmt.Println(a)
	}
	pool.Put(3)
	b, ok := pool.Get().(int)
	if ok {
		fmt.Println(b)
	}
}
```
####使用
适用与通过复用，降低复杂对象的创建和GC代价
协程安全，会有锁的开销
生命周期受GC影响，不适用于连接池等，需要自己管理生命周期的资源的池化
