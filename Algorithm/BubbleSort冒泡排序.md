冒泡排序是一种简单的排序算法。它适合小规模数据的排序,并且其效率比较低。冒泡排序是一种交换排序，它的基本思想是：两两比较相邻记录的关键字，如果反序则交换，直到没有反序的记录为止。
####算法步骤（以正序为例）
1.  比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2.  对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
3.  针对所有的元素重复以上的步骤，除了最后一个。
4.  持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

![bubbleSort.gif](https://upload-images.jianshu.io/upload_images/143845-5eac729e2deffeba.gif?imageMogr2/auto-orient/strip)

go实现
```
func bubbleSort(arr []int) []int {
	length := len(arr)
	for i := 0; i < length; i++ {
		for j := 0; j < length-1-i; j++ {
			if arr[j] > arr[j+1] {
				arr[j], arr[j+1] = arr[j+1], arr[j]
			}
		}
	}
	return arr
}
```
python
```
def bubbleSort(arr):
    for i in range(1, len(arr)):
        for j in range(0, len(arr)-i):
            if arr[j] > arr[j+1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr
```
####冒泡排序复杂度分析
分析一下它的时间复杂度。当最好的情况，也就是要排序的表本身就是有序的，那么我们比较次数，那么可以判断出就是n-1次的比较，没有数据交换，此时时间复杂度为O(n)。当最坏的情况，即待排序是逆序的情况，此时时间复杂度为 n ^2 。

