注：本文是《Go语言核心编程》（李文塔/著）个人读书笔记
####reflect.Type
```
type rtype struct {
	size       uintptr
	ptrdata    uintptr  // number of bytes in the type that can contain pointers
	hash       uint32   // hash of type; avoids computation in hash tables
	tflag      tflag    // extra type information flags
	align      uint8    // alignment of variable with this type
	fieldAlign uint8    // alignment of struct field with this type
	kind       uint8    // enumeration for C
	alg        *typeAlg // algorithm table
	gcdata     *byte    // garbage collection data
	str        nameOff  // string form
	ptrToThis  typeOff  // type for pointer to this type, may be zero
}
```
#####26种基础类型
```
const (
	Invalid Kind = iota
	Bool
	Int
	Int8
	Int16
	Int32
	Int64
	Uint
	Uint8
	Uint16
	Uint32
	Uint64
	Uintptr
	Float32
	Float64
	Complex64
	Complex128
	Array
	Chan
	Func
	Interface
	Map
	Ptr
	Slice
	String
	Struct
	UnsafePointer
)
```
####reflect. Value
reflect. Value 表示实例的值信息， reflect.Value 是一个 struct，并提供了一系列 的 method给使用者 。一个是值的类型指针 typ ，另 一个是指 向值的指针 ptr， 最后 一个是标记宇段 flag。
```
type Value struct {
	typ *rtype
        ptr unsafe.Pointer
	flag
}
```
####常用API
从实例到 Value 通过实例获取 Value 对象，直接使用 reflect.ValueOf（）函数
```
func ValueOf(i interface {})
```
从实例到 TypeValue通过实例获取反射对象的 Type，直接使用 reflect.Typeof()函数
```
func TypeOf(i interface{}) type
```
从 Type 到Value 
Type 里面只有类型信息，所以直接从一个Type 接口变量里面是无法获得实例的value的，但可以通过该Type构建一个新实例的Value 。reflect 包提供了两种方法。
```
func New(typ Type)Value
func Zero(typ Type) Value
```
如果知道一个类型值的底层存放地址，则还有一个函数是可以依据 type 和该地址值恢复出 Value 的
```
func NewAt(typ Type,p unsafe . Pointer ) Value
```
从 Value 到 Type 
从反射对象 Value 到 Type 可以直接调用 Value 的方法，因为 Value 内部存放着到 Type 类型 的指针。
```
func(v Value)Type()Type
```
####反射三定律
1反射可以从接口值得到反射对象 。
2反射可以从反射对象获得接口值。
3若要修改一个反射对象，则其值必须可以修改 。
