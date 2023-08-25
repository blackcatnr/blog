---
title: 数据结构
date: 2023-07-27
tags: ['星海']
categories: 星海
id: DS
---

# 数据结构

# 目录

- [二叉树](#section.2)<a name="context.2"> </a>
  - [遍历方式](#section.3)<a name="context.3"> </a>
    - [深度优先DFS（Depth First Search）](#section.4)<a name="context.4"> </a>
    - [广度优先BFS（Breath First Search）](#section.5)<a name="context.5"> </a>
  - [分类](#section.6)<a name="context.6"> </a>
- [二叉搜索树（BST）](#section.7)<a name="context.7"> </a>
- [AVL树（BBT）](#section.8)<a name="context.8"> </a>
  - [定义](#section.9)<a name="context.9"> </a>
  - [使用场景](#section.10)<a name="context.10"> </a>
  - [插入](#section.11)<a name="context.11"> </a>
    - [1.左左型右旋](#section.12)<a name="context.12"> </a>
    - [2.左右型的左右旋](#section.13)<a name="context.13"> </a>
    - [3.右右型左旋](#section.14)<a name="context.14"> </a>
    - [4.右左型右左旋](#section.15)<a name="context.15"> </a>
  - [总结](#section.16)<a name="context.16"> </a>
- [红黑树](#section.17)<a name="context.17"> </a>
  - [定义](#section.18)<a name="context.18"> </a>
------------------------------------------------

<!-- more -->

## [二叉树](#context.2)<a name="section.2"> </a>

为**有序树**：

如果结点的各子树从左到右是有次序的、不能颠倒，则为有序树，否则为无序树。对于有序树的孩子来说，最左边的孩子称为第一个孩子，最右边的孩子称为最后一个孩子。

比如，如果树T1是一个有序树，则其根结点的第一个孩子为结点B，最后一个孩子为结点D



### [遍历方式](#context.3)<a name="section.3"> </a>

#### [深度优先DFS（Depth First Search）](#context.4)<a name="section.4"> </a>

递归遍历 - 函数递归

```c++
class Tree;
void Recur(Tree *tree)
{
    if (tree != nullptr)
    {
        ///< 中左右
        cout << tree->val << endl;
        Recur(tree->left);
        Recur(tree->right);
    }
}

void DFSTree(Tree *tree)
{
    Recur(tree);
}


/* 统一遍历方法！！！ */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            if (node != NULL) {
                st.pop(); // 将该节点弹出，避免重复操作，下面再将右中左节点添加到栈中
                if (node->right) st.push(node->right);  // 添加右节点（空节点不入栈）

                st.push(node);                          // 添加中节点
                st.push(NULL); // 中节点访问过，但是还没有处理，加入空节点做为标记。

                if (node->left) st.push(node->left);    // 添加左节点（空节点不入栈）
            } else { // 只有遇到空节点的时候，才将下一个节点放进结果集
                st.pop();           // 将空节点弹出
                node = st.top();    // 重新取出栈中元素
                st.pop();
                result.push_back(node->val); // 加入到结果集
            }
        }
        return result;
    }
};
```



迭代遍历 - stack + while

```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();                       // 中
            st.pop();
            result.push_back(node->val);
            if (node->right) st.push(node->right);           // 右（空节点不入栈）
            if (node->left) st.push(node->left);             // 左（空节点不入栈）
        }
        return result;
    }
};
```



#### [广度优先BFS（Breath First Search）](#context.5)<a name="section.5"> </a>

层序遍历 - 队列

```c++
/* 迭代法 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        vector<vector<int>> result;
        while (!que.empty()) {
            int size = que.size();
            vector<int> vec;
            // 这里一定要使用固定大小size，不要使用que.size()，因为que.size是不断变化的
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        return result;
    }
};

/* 递归法 */
class Solution {
public:
    void order(TreeNode* cur, vector<vector<int>>& result, int depth)
    {
        if (cur == nullptr) return;
        if (result.size() == depth) result.push_back(vector<int>());
        result[depth].push_back(cur->val);
        order(cur->left, result, depth + 1);
        order(cur->right, result, depth + 1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        int depth = 0;
        order(root, result, depth);
        return result;
    }
};
```



### [分类](#context.6)<a name="section.6"> </a>

![image-20230719102948357](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230719102948357.png)

C++标准库是有多个版本的，要知道我们使用的STL是哪个版本，才能知道对应的栈和队列的实现原理。

那么来介绍一下，三个最为普遍的STL版本：

1. HP STL 其他版本的C++ STL，一般是以HP STL为蓝本实现出来的，HP STL是C++ STL的第一个实现版本，而且开放源代码。
2. P.J.Plauger STL 由P.J.Plauger参照HP STL实现出来的，被Visual C++编译器所采用，不是开源的。
3. SGI STL 由Silicon Graphics Computer Systems公司参照HP STL实现，被Linux的C++编译器GCC所采用，SGI STL是开源软件，源码可读性甚高。



栈提供push 和 pop 等等接口，所有元素必须符合先进后出规则，所以栈不提供走访功能，也不提供迭代器(iterator)。 不像是set 或者map 提供迭代器iterator来遍历所有元素。



**栈是以底层容器完成其所有的工作，对外提供统一的接口，底层容器是可插拔的（也就是说我们可以控制使用哪种容器来实现栈的功能）。**

所以STL中栈往往不被归类为容器，而被归类为container adapter（容器适配器）。



**我们常用的SGI STL，如果没有指定底层实现的话，默认是以deque为缺省情况下栈的底层结构。**

deque是一个双向队列，只要封住一段，只开通另一端就可以实现栈的逻辑了。



**SGI STL中 队列底层实现缺省情况下一样使用deque实现的。**

我们也可以指定vector为栈的底层实现，初始化语句如下：

```c
std::stack<int, std::vector<int> > third;  // 使用vector为底层容器的栈
```



刚刚讲过栈的特性，对应的队列的情况是一样的。

队列中先进先出的数据结构，同样不允许有遍历行为，不提供迭代器, **SGI STL中队列一样是以deque为缺省情况下的底部结构。**

也可以指定list 为起底层实现，初始化queue的语句如下：

```c
std::queue<int, std::list<int>> third; // 定义以list为底层容器的队列
```

所以STL 队列也不被归类为容器，而被归类为container adapter（ 容器适配器）。



## [二叉搜索树（BST）](#context.7)<a name="section.7"> </a>

也称**二叉查找树 BST（Binary Search Tree）**

二叉搜索树的**重要特质**为，**在二叉树的基础上增加了左子树的所有值应都小于根节点的值，右子树的所有值应都大于根节点的值**。

乍看二叉搜索树的访问效率很高，最大的遍历次数为树的高度，最小的遍历次数为1。但实际存在某种特殊情况，比如根节点只有一个右孩子，然后剩下的全部元素为一条直线向下排列在左子树，造成分部不均匀，查找和插入效率几乎等同于线性结构。如下图：

![image-20230717160027885](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230717160027885.png)

力扣参考题目：[96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)、[98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

```c++
/* 不同的二叉搜索树
给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。
 */
class Solution {
public:
    int numTrees(int n) {
        vector<int> g(n + 1, 0);
        g[0] = 1;
        g[1] = 1;

        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j <= i; ++j) {
                g[i] += g[j - 1] * g[i - j];
            }
        }

        return g[n];
    }
};

/* 验证二叉搜索树
给你一个二叉树的根节点 root ，判断其是否是一个有效的二叉搜索树。
 */
class Solution {
private:
    bool Recur(TreeNode *tree, long min, long max)
    {
        bool ret;
        if (!tree)
            ret = true;
        else if (tree->val <= min || tree->val >= max)
            ret = false;
        else
            ret = Recur(tree->left, min, tree->val) && Recur(tree->right, tree->val, max);

        return ret;
    }

public:
    bool isValidBST(TreeNode* root) {
        return Recur(root, LONG_MIN, LONG_MAX);
    }
};
```



## [AVL树（BBT）](#context.8)<a name="section.8"> </a>

**平衡二叉树**全称叫做 平衡二叉搜索（排序）树，简称 AVL树。英文：Balanced Binary Tree （BBT）

### [定义](#context.9)<a name="section.9"> </a>

AVL树本质上是一颗二叉搜索树，但它又具有以下特点：

1. 它是一颗空树或它的左右两个子树的高度差的绝对值不超过 1
2. 左右两个子树也是一颗平衡二叉树



**需满足以下特征：**

1. 对于任何一颗子树的root根结点而言，它的左子树任何节点的key一定比root小，而右子树任何节点的key 一定比root大；
2. 对于AVL树而言，其中任何子树仍然是AVL树；
3. 每个节点的左右子节点的高度之差的绝对值最多为1；



**平衡因子（BF -  Balance Factor） = 左子树的深度 - 右子树的深度**



### [使用场景](#context.10)<a name="section.10"> </a>

由于AVL树必须保证左右子树平衡，Max(最大树高-最小树高) <= 1，所以在插入的时候很容易出现不平衡的情况，一旦这样，就需要进行旋转以求达到平衡。

正是由于这种严格的平衡条件，导致AVL需要花大量时间在调整上，故AVL树一般使用场景在于**查询场景**， 而不是增加删除频繁的场景。



### [插入](#context.11)<a name="section.11"> </a>

#### [1.左左型右旋](#context.12)<a name="section.12"> </a>

![image-20230718152017647](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230718152017647.png)

在结点T的 **左结点（L）** 的 **左子树（L）** 上做了插入元素的操作，我们称这种情况为 **左左型** ，我们应该进行右旋转。



#### [2.左右型的左右旋](#context.13)<a name="section.13"> </a>

![image-20230718152120654](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230718152120654.png)

在结点T的 **左结点（L）** 的 **右子树（R）** 上做了插入元素的操作，我们称这种情况为 **左右型** ，我们应该进行左右旋。

**步骤如下：**

![image-20230718154009644](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230718154009644.png)



#### [3.右右型左旋](#context.14)<a name="section.14"> </a>

![image-20230718154115097](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230718154115097.png)

在结点T的 **右结点（R）** 的 **右子树（R）** 上做了插入元素的操作，我们称这种情况为 **右右型** ，我们应该进行左旋转。



#### [4.右左型右左旋](#context.15)<a name="section.15"> </a>

![image-20230718154205](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230718154205.png)

在结点T的 **右结点（R）** 的 **左子树（L）** 上做了插入元素的操作，我们称这种情况为 **右左型** ，我们应该进行右左旋。

**步骤如下：**

![image-20230718154226276](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/image-20230718154226276.png)



### [总结](#context.16)<a name="section.16"> </a>

| 插入位置                                              | 状态   | 操作   |
| ----------------------------------------------------- | ------ | ------ |
| 在结点T的左结点（L）的 **左子树（L）** 上做了插入元素 | 左左型 | 右旋   |
| 在结点T的左结点（L）的 **右子树（R）** 上做了插入元素 | 左右型 | 左右旋 |
| 在结点T的右结点（R）的 **右子树（R）** 上做了插入元素 | 右右型 | 左旋   |
| 在结点T的右结点（R）的 **左子树（L）** 上做了插入元素 | 右左型 | 右左旋 |





## [红黑树](#context.17)<a name="section.17"> </a>

红黑树（RBT）继承了AVL可自平衡的优点，

同时, 红黑树（RBT）在**查询速率和平衡调整**中寻找平衡，放宽了**树的平衡条件**，从而可以用于 **增加删除频繁**的场景。

在实际应用中，红黑树的使用要多得多。



**查找插入删除的时间复杂度为 O(logN)**



### [定义](#context.18)<a name="section.18"> </a>

**需满足以下特征：**

1. 节点非黑即红
2. 根节点一定是黑色
3. 叶子节点（NIL）一定是黑色
4. 每个红色节点的两个子节点都为黑色。(从每个叶子到根的所有路径上不能有两个连续的红色节点)
5. 从任一节点到其每个叶子的所有路径，都包含相同数目的黑色节点。

**注：默认插入节点颜色为红色**



