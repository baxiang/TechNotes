二分查找
```java
public static int findTwoPoint(int[] array, int key) {
		int start = 0;
		int last = array.length - 1;
		while (start <= last) {
			int mid = (last - start) / 2 + start;// 防止直接相加造成int范围溢出
			if (key == array[mid]) {// 查找值等于当前值，返回数组下标
				return mid;
			}
			if (key > array[mid]) {// 查找值比当前值大
				start = mid + 1;
			}
			if (key < array[mid]) {// 查找值比当前值小
				last = mid - 1;
			}
		}
		return -1;
	}
```
