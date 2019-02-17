sort 包 在内部实现了四种基本的排序算法：插入排序（insertionSort）、归并排序（symMerge）、堆排序（heapSort）和快速排序（quickSort）； sort 包会依据实际数据自动选择最优的排序算法。所以我们写代码时只需要考虑实现 sort.Interface 这个类型就可以了。
```
func Sort(data Interface) {
    // Switch to heapsort if depth of 2*ceil(lg(n+1)) is reached.
    n := data.Len()
    maxDepth := 0
    for i := n; i > 0; i >>= 1 {
        maxDepth++
    }
    maxDepth *= 2
    quickSort(data, 0, n, maxDepth)
}

type Interface interface {
    // Len is the number of elements in the collection.
    Len() int
    // Less reports whether the element with
    // index i should sort before the element with index j.
    Less(i, j int) bool
    // Swap swaps the elements with indexes i and j.
    Swap(i, j int)
}
// 内部实现的四种排序算法
// 插入排序
func insertionSort(data Interface, a, b int)
// 堆排序
func heapSort(data Interface, a, b int)
// 快速排序
func quickSort(data Interface, a, b, maxDepth int)
// 归并排序
func symMerge(data Interface, a, m, b int)
```
调用 sort.Sort() 来实现自定义类型排序，只需要我们的类型实现 Interface 接口类型中的三个方法即可。
##sort 包本身对于 []int 类型如何排序
```
type IntSlice []int
// 获取此 slice 的长度
func (p IntSlice) Len() int           { return len(p) }
// 比较两个元素大小 升序
func (p IntSlice) Less(i, j int) bool { return p[i] < p[j] }
// 交换数据
func (p IntSlice) Swap(i, j int)      { p[i], p[j] = p[j], p[i] }
// sort.Ints()内部调用Sort() 方法实现排序
// 注意 要先将[]int 转换为 IntSlice类型 因为此类型才实现了Interface的三个方法 
func Ints(a []int) { Sort(IntSlice(a)) }
```
``` go
package main

import (
	"fmt"
	"sort"
	"strings"
)

type StuScore struct {
	name  string
	score int
}

type StuScores []StuScore

func (s StuScores) Len() int {
	return len(s)
}

func (s StuScores) Less(x, y int) bool {
	return s[x].score < s[y].score
}
func (s StuScores) Swap(x, y int) {
	s[x], s[y] = s[y], s[x]
}

type SortByName struct {
	StuScores
}

func (s SortByName) Less(x, y int) bool {
	if strings.Compare(s.StuScores[x].name, s.StuScores[y].name) < 1 {
		return true
	}
	return false
}

type SortByAgeDESC struct {
	StuScores
}

func (s SortByAgeDESC) Less(x, y int) bool {
	return s.StuScores[x].score > s.StuScores[y].score
}

func main() {
	stus := StuScores{{"name2", 95}, {"name1", 75}, {"name5", 86}, {"name4", 60}, {"name3", 100}}
	fmt.Println("排序前------------------")
	for _, v := range stus {
		fmt.Println(v.name, ":", v.score)
	}
	fmt.Println("按照分数升序排序------------------")
	sort.Sort(stus)
	for _, v := range stus {
		fmt.Println(v.name, ":", v.score)
	}
	fmt.Println("按照姓名排序------------------")
	sort.Sort(SortByName{stus})
	for _, v := range stus {
		fmt.Println(v.name, ":", v.score)
	}
	fmt.Println("按照分数降序排序------------------")
	sort.Sort(SortByAgeDESC{stus})
	for _, v := range stus {
		fmt.Println(v.name, ":", v.score)
	}
}

```
当然不是。我们可以利用嵌套结构体来解决这个问题。因为嵌套结构体可以继承父结构体的所有属性和方法;
```

type SortByName struct {
	StuScores
}

func (s SortByName) Less(x, y int) bool {
	if strings.Compare(s.StuScores[x].name, s.StuScores[y].name) < 1 {
		return true
	}
	return false
}

type SortByAgeDESC struct {
	StuScores
}

func (s SortByAgeDESC) Less(x, y int) bool {
	return s.StuScores[x].score > s.StuScores[y].score
}

```
