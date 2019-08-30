## 1 算法步骤
1.  首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置
2.  再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
3.  重复第二步，直到所有元素均排序完毕。

![selectionSort.gif](https://upload-images.jianshu.io/upload_images/143845-86c8329b47a18e55.gif?imageMogr2/auto-orient/strip)
#### Python 代码实现
```source-python
def selectionSort(arr):
    for i in range(len(arr) - 1):
        # 记录最小数的索引
        minIndex = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[minIndex]:
                minIndex = j
        # i 不是最小数时，将 i 和最小数进行交换
        if i != minIndex:
            arr[i], arr[minIndex] = arr[minIndex], arr[i]
    return arr
```
#### Go 代码实现
```source-go
func selectionSort(arr []int) []int {
	length := len(arr)
	for i := 0; i < length-1; i++ {
		min := i
		for j := i + 1; j < length; j++ {
			if arr[min] > arr[j] {
				min = j
			}
		}
		arr[i], arr[min] = arr[min], arr[i]
	}
	return arr
}
```

####Java 代码实现
```java
 public  static int [] selectSort(int[] sourceArray) throws Exception{
        int[] arr = Arrays.copyOf(sourceArray,sourceArray.length);
        //总共需要进行N-1轮比较
        for (int i=0;i<arr.length-1;i++){
            int min = i;
            // 每轮需要后面的元素进行比较
            for(int j = i+1;j<arr.length;j++){
                if (arr[min]>arr[j])
                    min = j;
            }
            //将找到的最小值和当前坐标进行交换
            if (i!=min){
                int tmp = arr[i];
                arr[i] = arr[min];
                arr[min] = tmp;
            }
        }
        return arr;
    }
```
