####概念
**树**（英语：tree）是一种[抽象数据类型](https://zh.wikipedia.org/wiki/%E6%8A%BD%E8%B1%A1%E8%B3%87%E6%96%99%E5%9E%8B%E5%88%A5 "抽象数据类型")（ADT）或是实现这种抽象数据类型的[数据结构](https://zh.wikipedia.org/wiki/%E8%B3%87%E6%96%99%E7%B5%90%E6%A7%8B "数据结构")，用来模拟具[有树状结构](https://zh.wikipedia.org/wiki/%E6%A8%B9%E7%8B%80%E7%B5%90%E6%A7%8B "树状结构")性质的数据集合。它是由n（n>0）个有限节点组成一个具有层次关系的[集合](https://zh.wikipedia.org/wiki/%E9%9B%86%E5%90%88 "集合")。把它叫做“树”是因为它看起来像一棵倒挂的树，也就是说它是根朝上，而叶朝下的。
**特点**
*   每个节点都只有有限个子节点或无子节点；
*   没有父节点的节点称为根节点；
*   每一个非根节点有且只有一个父节点；
*   除了根节点外，每个子节点可以分为多个不相交的子树；
*   树里面没有环路(cycle)

####术语
节点的度：一个节点含有的子树的个数称为该节点的度；
树的度：一棵树中，最大的节点度称为树的度；
叶节点或终端节点：度为零的节点；
高度Height 节点到叶子节点的最长路径，树的高度等于根节点的高度。
深度Depth 根节点到这个节点的所尽力的边的个数
层Level：节点的深度+1
父亲节点或父节点：若一个节点含有子节点，则这个节点称为其子节点的父节点；
叶子结点(Leaves)：是树的末端结点，他们没有子结点，就像真实的树那样 ，由根开始，伸展枝干，到叶为止。

![image.png](https://upload-images.jianshu.io/upload_images/143845-ff3e80020fb3ab90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####树的生活映射
**家族关系**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-377d751896dfcca6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**公司组织结构**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-5024fefb74528835.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**html结构**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-93ca96f19681dd01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####解决的问题
 **效率**
![图片.png](https://upload-images.jianshu.io/upload_images/143845-e3875eadc8f6cf69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这个索引可以把原本O(n)的查找操作变为O(logn)，可以简单地理解为在数据结构的层面上构造了一个二分查找算法。总之，树通过其结构来表达了一种划分查找方法，这一方法相比于遍历搜索的复杂度O(n)，一般情况下复杂度仅有O(logn)。

 **稳定**
如果我们仅仅考虑效率问题，那散列比树要屌的多。查找复杂度为O(1)。之所以不能用散列来取代树，是因为散列需要预先开辟大量空间，并不是所有场景下都可以这么做；而如果空间不够，则会出现散列冲突（索引结构被破坏）。树则可以动态改变存储空间，且运用一些手段来保护自身索引结构。

自平衡二叉搜索树 (self-balanced BST)的精髓，便在于其维护自身稳定（平衡）的能力。当树不平衡时，其搜索复杂度便不再是O(logn)了——考虑一个极端情况：一棵树每个节点都只有右子节点，那么树就退化为了链表，查找复杂度也和链表一样是O(n)：

![一棵退化的树](https://upload-images.jianshu.io/upload_images/143845-ccd7e7bccd2b21a6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####树结构表示
python实现
```python
class TreeNode:
    def __init__(self,val):
        self.val = val
        self.left,self.right = None,None
```
Java实现
``` java
public class TreeNode {
    
    public int val;
    public TreeNode left,right;
    public TreeNode(int val){
        this.left = null;
        this.right = null;
        this.val = val;
    }
}

```
层序遍历 通过队列实现
```
 // 层序遍历
    public void levelOrder(){
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()){
            Node curNode = queue.remove();
            System.out.println(curNode.data);
            if (curNode.left!=null){
                queue.add(curNode.left);
            }
            if (curNode.right!=null){
                queue.add(curNode.right);
            }
        }

    }
```



