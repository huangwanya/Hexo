---
title: AVL实现 
date: 2020-03-13 18:54:47
tags: java
categories: 数据结构
---
## 前言：
前面是Avl树的介绍写的比较详细，这一篇主要写怎么实现
## 最简单的旋转

依次插入1 2 3节点，1的左子树为空高度为0，而右子树高度为2，旋转后，左右高度都为1
![](https://s2.ax1x.com/2020/03/06/3LAHAg.png)

## 单旋转

依次插入6 3 7 1 4，插入2时，树的平衡被破坏
![](https://s2.ax1x.com/2020/03/06/3LAbNQ.png)
![](https://s2.ax1x.com/2020/03/06/3LAqhj.png)
### 步骤：

获取k1节点=k2的左边节点
设置k2的左边节点为k1的右边节点Y
设置k1的右边节点为k2
重新计算k2和k1的高度
```java
 private AvlNode<T> rotateWithLeftChild(AvlNode<T> k2) {
        AvlNode k1 = k2.left;
        k2.left = k1.right;
        k1.right = k2;
        k2.height = Math.max(height(k2.left), height(k2.right)) + 1;
        k1.height = Math.max(height(k1.left), k2.height) + 1;
        return k1;
    }

```
## 双旋转

依次插入6 2 7 1 4，插入3时，树的平衡被破坏
![](https://s2.ax1x.com/2020/03/06/3LATHS.png)
下面的图解C节点实际上应该没有，因为插入B的时候已经影响平衡
![](https://s2.ax1x.com/2020/03/06/3LAoB8.png)

### 步骤：

k3的左边子树进行一次右边的单旋转
k3进行一次左边的单旋转
```java
 private AvlNode<T> doubleWithLeftChild(AvlNode<T> k3) {
        k3.left = rotateWithRightChild(k3.left);
        return rotateWithLeftChild(k3);
    }

```
### 实现

平衡二叉树实现的大部分过程和二叉查找树是一样的，区别就在于插入和删除之后要判断树左右子树高度之差是否大于1，如果大于1需要进行旋转维持平衡
其它的两种插入影响平衡的情况和以上两种刚好相反

## 代码
## 树节点
```java
package 二叉树;
public class AvlNode<T> {
    T element;
    int height;
    AvlNode<T> left;
    AvlNode<T> right;

    public AvlNode(T element) {
        this(element, null, null);
    }

    public AvlNode(T element, AvlNode<T> left, AvlNode<T> right) {
        this.element = element;
        this.left = left;
        this.right = right;
        this.height = 0;
    }
}

```
## 平衡代码
```java
private AvlNode<T> balance(AvlNode<T> t) {
        if (t == null) {
            return t;
        }

        if (height(t.left) - height(t.right) > ALLOWED_IMBALANCE) {
            if (height(t.left.left) >= height(t.left.right)) {
                t = rotateWithLeftChild(t);
            } else {
                t = doubleWithLeftChild(t);
            }
        } else if (height(t.right) - height(t.left) > ALLOWED_IMBALANCE) {
            if (height(t.right.right) >= height(t.right.left)) {
                t = rotateWithRightChild(t);
            } else {
                t = doubleWithRightChild(t);
            }
        }
        t.height = Math.max(height(t.left), height(t.right)) + 1;
        return t;
    }

```
## 完整代码
```java
package 二叉树;

/**
 *
 * @author 皖
 *
 * @param <T>
 */
public class AvlTree<T extends Comparable<? super T>> {

    private AvlNode<T> root;

    public void insert(T x) {
        root = insert(x, root);
    }

    public void remove(T x) {
        root = remove(x, root);
    }

    public T findMin() {
        return findMin(root).element;
    }


    public void makeEmpty() {
        root = null;
    }

    public boolean isEmpty() {
        return root == null;
    }

    /**
     * 添加节点
     *
     * @param x 插入节点
     * @param t 父节点
     */
    private AvlNode<T> insert(T x, AvlNode<T> t) {

        //如果根节点为空，则当前x节点为根及诶单
        if (null == t) {
            return new AvlNode(x);
        }

        int compareResult = x.compareTo(t.element);

        //小于当前根节点 将x插入根节点的左边
        if (compareResult < 0) {
            t.left = insert(x, t.left);
        } else if (compareResult > 0) {
            //大于当前根节点 将x插入根节点的右边
            t.right = insert(x, t.right);
        } else {

        }
        return balance(t);
    }

    private static final int ALLOWED_IMBALANCE = 1;

    private AvlNode<T> balance(AvlNode<T> t) {
        if (t == null) {
            return t;
        }

        if (height(t.left) - height(t.right) > ALLOWED_IMBALANCE) {
            if (height(t.left.left) >= height(t.left.right)) {
                t = rotateWithLeftChild(t);
            } else {
                t = doubleWithLeftChild(t);
            }
        } else if (height(t.right) - height(t.left) > ALLOWED_IMBALANCE) {
            if (height(t.right.right) >= height(t.right.left)) {
                t = rotateWithRightChild(t);
            } else {
                t = doubleWithRightChild(t);
            }
        }
        t.height = Math.max(height(t.left), height(t.right)) + 1;
        return t;
    }

    private AvlNode<T> doubleWithRightChild(AvlNode<T> k3) {
        k3.right = rotateWithLeftChild(k3.right);
        return rotateWithRightChild(k3);
    }

    private AvlNode<T> rotateWithRightChild(AvlNode<T> k2) {
        AvlNode k1 = k2.right;
        k2.right = k1.left;
        k1.left = k2;
        k2.height = Math.max(height(k2.right), height(k2.left)) + 1;
        k1.height = Math.max(height(k1.right), k2.height) + 1;
        return k1;
    }

    private AvlNode<T> doubleWithLeftChild(AvlNode<T> k3) {
        k3.left = rotateWithRightChild(k3.left);
        return rotateWithLeftChild(k3);
    }

    private AvlNode<T> rotateWithLeftChild(AvlNode<T> k2) {
        AvlNode k1 = k2.left;
        k2.left = k1.right;
        k1.right = k2;
        k2.height = Math.max(height(k2.left), height(k2.right)) + 1;
        k1.height = Math.max(height(k1.left), k2.height) + 1;
        return k1;
    }

    private int height(AvlNode<T> t) {
        return t == null ? -1 : t.height;
    }

    /**
     * 删除节点
     *
     * @param x    节点
     * @param t    父节点
     */
    private AvlNode<T> remove(T x, AvlNode<T> t) {

        if (null == t) {
            return t;
        }

        int compareResult = x.compareTo(t.element);

        //小于当前根节点
        if (compareResult < 0) {
            t.left = remove(x, t.left);
        } else if (compareResult > 0) {
            //大于当前根节点
            t.right = remove(x, t.right);
        } else if (t.left != null && t.right != null) {
            //找到右边最小的节点
            t.element = findMin(t.right).element;
            //当前节点的右边等于原节点右边删除已经被选为的替代节点
            t.right = remove(t.element, t.right);
        } else {
            t = (t.left != null) ? t.left : t.right;
        }
        return balance(t);
    }

    /**
     * 找最小节点
     *
     * @param root 根节点
     */
    private AvlNode<T> findMin(AvlNode<T> root) {
        if (root == null) {
            return null;
        } else if (root.left == null) {
            return root;
        }
        return findMin(root.left);
    }

    /**
     * 找最大节点
     *
     * @param root 根节点
     */
    private AvlNode<T> findMax(AvlNode<T> root) {
        if (root == null) {
            return null;
        } else if (root.right == null) {
            return root;
        } else {
            return findMax(root.right);
        }
    }

    public void printTree() {
        if (isEmpty()) {
            System.out.println("节点为空");
        } else {
            printTree(root);
        }
    }


    public void printTree(AvlNode<T> root) {
        if (root != null) {
            System.out.print(root.element);
            if (null != root.left) {
                System.out.print("左边节点" + root.left.element);
            }
            if (null != root.right) {
                System.out.print("右边节点" + root.right.element);
            }
            System.out.println();
            printTree(root.left);
            printTree(root.right);
        }


    }

}

```
## 测试
```java
package 二叉树;

public class Test{
    public static void main (String[] args) {
            AvlTree testTree = new AvlTree();
            testTree.insert(36);
            testTree.insert(22);
            testTree.insert(16);
            testTree.insert(45);
            testTree.insert(28);
            testTree.insert(60);

            testTree.printTree();


    }
}

```
