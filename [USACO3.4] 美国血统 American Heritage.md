# [USACO3.4] 美国血统 American Heritage

## 题目描述

农夫约翰非常认真地对待他的奶牛们的血统。然而他不是一个真正优秀的记帐员。他把他的奶牛 们的家谱作成二叉树，并且把二叉树以更线性的“树的中序遍历”和“树的前序遍历”的符号加以记录而 不是用图形的方法。

你的任务是在被给予奶牛家谱的“树中序遍历”和“树前序遍历”的符号后，创建奶牛家谱的“树的 后序遍历”的符号。每一头奶牛的姓名被译为一个唯一的字母。（你可能已经知道你可以在知道树的两 种遍历以后可以经常地重建这棵树。）显然，这里的树不会有多于 $26$ 个的顶点。

这是在样例输入和样例输出中的树的图形表达方式：


```plain
　　　　　　　　 C
　　　　　　   /  \
　　　　　　  /　　 \
　　　　　　 B　　   G
　　　　　　/ \　 　/
　　　　   A   D  H
　　　　　　  / \
　　　　　　 E   F

```

附注：

- 树的中序遍历是按照左子树，根，右子树的顺序访问节点；
- 树的前序遍历是按照根，左子树，右子树的顺序访问节点；
- 树的后序遍历是按照左子树，右子树，根的顺序访问节点。

## 输入格式

第一行一个字符串，表示该树的中序遍历。

第二行一个字符串，表示该树的前序遍历。

## 输出格式

单独的一行表示该树的后序遍历。

## 样例 #1

### 样例输入 #1

```
ABEDFCHG
CBADEFGH
```

### 样例输出 #1

```
AEFDBHGC
```

## 提示

题目翻译来自NOCOW。

USACO Training Section 3.4



### 二叉树的构造与后序遍历

1. 定义函数`Treenode *buidlBinaryTree(string inOrder, string preOrder)`：输中序字符串与前序字符串，返回二叉树节点；

2. 函数的具体实现：通过前序字符串与中序字符串，找到根节点的值：前序字符串的第一位；新建根节点`Treenode *root = new Treenode(inOrder[0]);`；找到根节点的左右节点的前序与中序字符串，双指针法，遍历中序与前序字符串；

3. 后序遍历二叉树，输出结果；



```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    char val;
    TreeNode *left, *right;
    TreeNode(const char x) : val(x), left(nullptr), right(nullptr) {
    }
};

TreeNode *buildTree(const string &inOrder, const string &preOrder);

void print(TreeNode *root);
void clean(TreeNode *root);

int main() {
    string inOrder, preOrder;
    cin >> inOrder >> preOrder;
    TreeNode *root = buildTree(inOrder, preOrder);
    print(root);
    clean(root);
    return 0;
}

TreeNode *buildTree(const string &inOrder, const string &preOrder) {
    if (inOrder.empty()) {
        return nullptr;
    }
    TreeNode *root = new TreeNode(preOrder[0]);
    string leftInOrder, leftPreOrder, rightInOrder, rightPreOrder;
    bool flag = true;
    for (int i = 0; i < inOrder.length(); i++) {
        if (inOrder[i] == root->val) {
            flag = false;
            continue;
        }
        if (flag) {
            leftInOrder += inOrder[i];
            leftPreOrder += preOrder[i + 1];
        } else {
            rightInOrder += inOrder[i];
            rightPreOrder += preOrder[i];
        }
    }
    root->left = buildTree(leftInOrder, leftPreOrder);
    root->right = buildTree(rightInOrder, rightPreOrder);
    return root;
}

void print(TreeNode *root) {
    if (root == nullptr) {
        return;
    }
    print(root->left);
    print(root->right);
    printf("%c", root->val);
}

void clean(TreeNode *root) {
    if (root == nullptr) {
        return;
    }
    clean(root->left);
    clean(root->right);
    delete root;
}
```