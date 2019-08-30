####递归定义
编程的角度来看，程序调用自身的编程技巧称为递归（recursion）。
本质上将原来的问题转化成更小的同一问题。就是一个函数直接或间接调用自身的一种方法，它通常把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解。
####递归必须要有三个要素：
1、边界条件
2、递归前进段
3、递归返回段
当边界条件不满足时，递归前进；当边界条件满足时，递归返回。
```
int func(传入数值) {
  if (终止条件) return 最小子问题解;
  return func(缩小规模);
}
```
#####阶乘
```
    public static int getFactorial(int n) {
        int temp = 1;
        if (n >= 0) {
            for (int i = 1; i <= n; i++) {
                temp = temp * i;
            }
        } else {
            return -1;
        }
        return temp;
    }
```
递归表示
```
public static int getFactorial(int n) {
        if (n==0){
            System.out.println(n + "!=1");
            return 1;
        }
        if (n>0){
           System.out.println(n);
           int temp = n*getFactorial(n-1);
           System.out.println(n + "!=" + temp);
           return temp;
        }
        return -1;
    }
```
n相加递归表达
```
    public static int getFactorial(int n) {
        if (n==0){
            return 0;
        }
        return n+getFactorial(n-1);
    }
```

####递归与栈的关系
递归的过程就是出入栈的过程
![image](https://upload-images.jianshu.io/upload_images/143845-69db1eb40a6a96c8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
第 1~4 步，都是入栈过程，`Factorial(3)`调用了`Factorial(2)`，`Factorial(2)`又接着调用`Factorial(1)`，直到`Factorial(0)`；
 第 5 步，因 0 是递归结束条件，故不再入栈，此时栈高度为 4，即为我们平时所说的递归深度；
第 6~9 步，`Factorial(0)`做完，出栈，而`Factorial(0)`做完意味着`Factorial(1)`也做完，同样进行出栈，重复下去，直到所有的都出栈完毕，递归结束。
每一个递归程序都可以把它改写为非递归版本。我们只需利用栈，通过入栈和出栈两个操作就可以模拟递归的过程，二叉树的遍历无疑是这方面的代表。
但是并不是每个递归程序都是那么容易被改写为非递归的。某些递归程序比较复杂，其入栈和出栈非常繁琐，给编码带来了很大难度，而且易读性极差，所以条件允许的情况下，推荐使用递归。
##### 汉诺塔
![image](https://upload-images.jianshu.io/upload_images/143845-86fdcb1166be90b4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
问题描述为：有三根杆子 A，B，C。A 杆上有 N 个穿孔圆盘，盘的尺寸由上到下依次变大，B，C 杆为空。要求按下列规则将所有圆盘移至 C 杆：
1.  每次只能移动一个圆盘；
2.  大盘不能叠在小盘上面。
问：如何移？最少要移动多少次？
首先看下基本情况，即终止条件：N=1 时，直接从 A 移到 C。
再来看下通用情况：当有 N 个圆盘在 A 上，我们已经找到办法将其移到 C 杠上了，我们怎么移动 N+1 个圆盘到 C 杠上呢？很简单，我们首先用将 N 个圆盘移动到 C 上的方法将 N 个圆盘都移动到 B 上，然后再把第 N+1 个圆盘（最后一个）移动到 C 上，再用同样的方法将在 B 杠上的 N 个圆盘移动到 C 上，问题解决。
```
 void Hanoi(int n, char a, char b, char c){    //终止条件
   if (n == 1)
    {
         cout << a << "-->" << c << endl;        

         return;
 
    }    //通用情况
 
    Hanoi(n - 1, a, c, b);
 
    Hanoi(1, a, b, c);

    Hanoi(n - 1, b, a, c);
 }
```
##### 求二叉树节点个数展开目录

![image](https://upload-images.jianshu.io/upload_images/143845-eea98807112b1c96?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先看下基本情况，即终止条件：当为空树时，节点数为 0；
再来看下通用情况：当前节点的左，右子树节点数都被求出，则以当前结点为根的二叉树的节点总数就是 “左子树 + 右子树 + 1”。

代码如下：
```
 int GetNodes(Node * node){    //终止条件
  if (node == nullptr)
    return 0;    //通用情况
    return GetNodes(node->left) + GetNode(node->right) + 1;
  }
```
####堆栈溢出
就是不顾堆栈中分配的局部数据块大小，向该数据块写入了过多的数据，导致数据越界，结果覆盖了别的数据。 可以理解为 在长字符串中嵌入一段代码，并将过程的[返回地址](https://baike.baidu.com/item/%E8%BF%94%E5%9B%9E%E5%9C%B0%E5%9D%80)覆盖为这段代码的地址，这样当过程返回时，程序就转而开始执行这段自编的代码了。
