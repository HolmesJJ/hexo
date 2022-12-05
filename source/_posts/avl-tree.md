---
title: AVL树
---

### 入门链接

[五分钟告诉你什么是平衡二叉树 ？什么是二叉排序树？平衡因子的计算等等](https://www.bilibili.com/video/BV1d7411u79x)<br>
[平衡二叉排序树|AVL树-从入门到入土](https://www.bilibili.com/video/BV1e4411x7rZ)<br>
[二叉平衡树AVL平衡调整数据结构](https://www.bilibili.com/video/BV1xE411h7dd)<br>
[平衡二叉树（AVL树） 调整 大一统 极简方法讲解](https://www.bilibili.com/video/BV13J411v73t)

### 详解

[什么是平衡二叉树（AVL）](https://zhuanlan.zhihu.com/p/56066942)<br>
[java数据结构与算法之平衡二叉树(AVL树)的设计与实现](https://blog.csdn.net/javazejian/article/details/53892797)<br>
[示例代码](https://github.com/shinezejian/javaStructures)<br>
[图解：什么是AVL树？（删除总结篇）](https://blog.csdn.net/been123456789jimmy/article/details/106515472)<br>
[Difference between the time complexity required to build Binary search tree and AVL tree?](https://stackoverflow.com/questions/17629668/difference-between-the-time-complexity-required-to-build-binary-search-tree-and)

### 平衡操作（分类和旋转）

<img width="788" alt="avl_tree" src="https://user-images.githubusercontent.com/62185825/110244053-e729d600-7f97-11eb-873b-e88b98cb6d1f.jpg"><br>
<img width="788" alt="avl_tree" src="https://user-images.githubusercontent.com/62185825/110244086-1b9d9200-7f98-11eb-9998-7d5dfbeb7c55.jpg"><br>
<img width="788" alt="avl_tree" src="https://user-images.githubusercontent.com/62185825/110244139-599ab600-7f98-11eb-829f-66c3573ddc3a.jpg"><br>
<img width="788" alt="avl_tree" src="https://user-images.githubusercontent.com/62185825/110244169-746d2a80-7f98-11eb-9e42-587aeea57b9f.jpg">

### 标准写法

```java
public class AVLNode<T> {

    // 左结点
    public AVLNode<T> left;

    // 右结点
    public AVLNode<T> right;

    // 存储的数据
    public T data;

    // 当前结点的高度
    public int height;

    // 一个没有叶节点的节点
    public AVLNode(T data) {
        this(null, null, data);
    }

    // 一个有叶节点的节点，且当前节点的高度为0
    public AVLNode(AVLNode<T> left, AVLNode<T> right, T data) {
        this(left, right, data, 0);
    }

    // 一个有叶节点的特定高度的节点
    public AVLNode(AVLNode<T> left, AVLNode<T> right, T data, int height) {
        this.left = left;
        this.right = right;
        this.data = data;
        this.height = height;
    }
}
```

### AVL树高度和结点的关系（详见Tutorial 4 Problem 1.b.）

**证明：一个有n个结点的AVL树的最高的高度h < 2log(n)**

* 设**n是高度为h的AVL树的最少结点数**，我们可以得到 **n(h) = n(h-1) + n(h-2) + 1**
* 注意：斐波那契数列F(n)中**后一数字n与前一数字n-1之比趋近于以固定常数1.618**，由此可以得到一个F与n的近似的递推关系式 （[参考链接](https://www.hmarkets.mu/zh-hans/fibonacci/)）
* [算法笔记：平衡二叉树（AVL）最小结点数与斐波那契数列的关系](https://blog.csdn.net/weixin_30414155/article/details/98193281)
