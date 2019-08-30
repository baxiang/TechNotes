####概述
二叉树(binary tree),是每个结点最多有2个的有序树(tree） 。
简单的理解，就是一种类似于我们生活中树的结构，只不过每个结点上都最多只能有两个子结点。顶上的叫根结点，两边被称作“左子树”(left child)和“右子树”(right child)。
二叉树具有唯一的根节点
二叉树每个节点最多有两个孩子
二叉树每个节点最多有一个父亲节点，根节点是唯一没有父亲节点的。
二叉树具有天然递归结构。
每个节点的子树都可以看做是二叉树。
![image.png](https://upload-images.jianshu.io/upload_images/143845-92a68026c2cd6cc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####满二叉树
一个二叉树所有非叶子节点都存在左右子树，并且所有的叶子节点都在同一层级上
####完全二叉树
####物理存储
链式存储
每个节点包含三个部分，存储数据的data变量，指向左孩子的left指针，指向右孩子的right指针
```
 class Node {
    private Object data;
    private Node left;
    private Node right;
}
```
####二叉树存储方式
基于指针或者引用的二叉链式存储
![image.png](https://upload-images.jianshu.io/upload_images/143845-4f70fc500b43a62d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

基于数组的顺序存储
![image.png](https://upload-images.jianshu.io/upload_images/143845-392479ffad9f5e84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
按照层级顺序把二叉树的节点放到数组对应的位置上，如果某一个节点的左孩子或右孩子空缺，则数组对应的位置也空出来。
父节点的下标是parent,那么它的左孩子节点下标是2*parent+1;右孩子节点下标是就是2*parnet+2

####二叉树遍历
二叉树是典型的非线性数据结构，遍历时需要把非线性关联的节点转成一个线性的序列。
######前序遍历
输出顺序是根节点，左子树，右子树。
######中序遍历
输出顺序是左子树，根节点，右子树
######后序遍历
输出顺序是左子树，右子树，根节点
