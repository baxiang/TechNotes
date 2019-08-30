####BST概述
二分搜索树是二叉树
二分搜索树的每个节点的值：
1左子树上所有的节点的值都小于它的根节点的值。
2右子树上所有的节点的值均小于它的根节点的值。
每一颗子树也是二分搜索树。
![image.png](https://upload-images.jianshu.io/upload_images/143845-a9e5ad598480b6c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


二分搜索树 递归实现
```
    public void add(E e){
        root = add(root,e);
    }
    
    /**
     * 二分搜索树插入元素 递归实现
     */
    private Node add(Node node ,E e){
        if (node==null){
            size++;
            return new Node(e);
        }
        if (e.compareTo(node.data)<0){
           node.left = add(node.left,e);
        }else if (e.compareTo(node.data)>0){
            node.right = add(node.right,e);
        }
        return node;
    }
```
二分搜索树 查找递归实现
```
public boolean contains(E e){
        return contains(root, e);
    }
    private boolean contains(Node node,E e){
        if (node==null){
            return false;
        }if (e.compareTo(node.data)==0){
            return true;
        }else if(e.compareTo(node.data)<0){
            return contains(node.left,e);
        }else {
            return contains(node.right,e);
        }
    }
```
####二叉树的遍历
`前序遍历`:对于树中的任意节点来说，先打印这个节点，然后再打印它的左子树，最后打印它的右子树。
` 中序遍历`:对于树中的任意节点来说，先打印它的左子树，然后再打印它本身，最后打印它的右子树。 
`后序遍历`:对于树中的任意节点来说，先打印它的左子树，然后再打印它的右子树，最后打印这个节点本身。
前序遍历
```
    public void preOrder(Node node){
        if (node==null){
            return;
        }
        System.out.println(node.data);
        preOrder(node.left);
        preOrder(node.right);
    }
```
中序遍历
```
 private void inOrder(Node node) {
        if (node == null) {
            return;
        }
        preOrder(node.left);
        System.out.println(node.data);
        preOrder(node.right);
    }
```
