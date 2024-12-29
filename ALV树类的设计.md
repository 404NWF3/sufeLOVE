# ALV树类的设计

1. **节点结构**：包含值、左子节点、右子节点和高度信息。
2. **插入操作**：先插入，再调整树以保持自平衡。
3. **删除操作**：删除节点，并调整树的平衡。
4. **查找操作**：标准二叉搜索树查找。

```cpp
#include <iostream>
using namespace std;

class AVLNode {
public:
    int value;
    AVLNode* left;
    AVLNode* right;
    int height;

    AVLNode(int val) : value(val), left(nullptr), right(nullptr), height(1) {}
};

class AVLTree {
private:
    AVLNode* root;

    int getHeight(AVLNode* node) {
        return node ? node->height : 0;
    }

    int getBalance(AVLNode* node) {
        return node ? getHeight(node->left) - getHeight(node->right) : 0;
    }

    AVLNode* rightRotate(AVLNode* y) {
        AVLNode* x = y->left;
        AVLNode* T2 = x->right;

        // Perform rotation
        x->right = y;
        y->left = T2;

        // Update heights
        y->height = max(getHeight(y->left), getHeight(y->right)) + 1;
        x->height = max(getHeight(x->left), getHeight(x->right)) + 1;

        return x;
    }

    AVLNode* leftRotate(AVLNode* x) {
        AVLNode* y = x->right;
        AVLNode* T2 = y->left;

        // Perform rotation
        y->left = x;
        x->right = T2;

        // Update heights
        x->height = max(getHeight(x->left), getHeight(x->right)) + 1;
        y->height = max(getHeight(y->left), getHeight(y->right)) + 1;

        return y;
    }

    AVLNode* insert(AVLNode* node, int value) {
        // Normal BST insert
        if (!node) return new AVLNode(value);
        if (value < node->value) node->left = insert(node->left, value);
        else if (value > node->value) node->right = insert(node->right, value);
        else return node; // No duplicates

        // Update height of this ancestor node
        node->height = 1 + max(getHeight(node->left), getHeight(node->right));

        // Check balance and rotate
        int balance = getBalance(node);

        // Left Left Case
        if (balance > 1 && value < node->left->value) {
            return rightRotate(node);
        }
        // Right Right Case
        if (balance < -1 && value > node->right->value) {
            return leftRotate(node);
        }
        // Left Right Case
        if (balance > 1 && value > node->left->value) {
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }
        // Right Left Case
        if (balance < -1 && value < node->right->value) {
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }

        return node;
    }

    AVLNode* minValueNode(AVLNode* node) {
        AVLNode* current = node;
        while (current->left != nullptr) current = current->left;
        return current;
    }

    AVLNode* deleteNode(AVLNode* root, int value) {
        // Normal BST delete
        if (!root) return root;
        if (value < root->value) root->left = deleteNode(root->left, value);
        else if (value > root->value) root->right = deleteNode(root->right, value);
        else {
            if (!root->left) return root->right;
            else if (!root->right) return root->left;

            AVLNode* temp = minValueNode(root->right);
            root->value = temp->value;
            root->right = deleteNode(root->right, temp->value);
        }

        // Update height of this ancestor node
        root->height = max(getHeight(root->left), getHeight(root->right)) + 1;

        // Check balance and rotate
        int balance = getBalance(root);

        // Left Left Case
        if (balance > 1 && getBalance(root->left) >= 0) {
            return rightRotate(root);
        }
        // Left Right Case
        if (balance > 1 && getBalance(root->left) < 0) {
            root->left = leftRotate(root->left);
            return rightRotate(root);
        }
        // Right Right Case
        if (balance < -1 && getBalance(root->right) <= 0) {
            return leftRotate(root);
        }
        // Right Left Case
        if (balance < -1 && getBalance(root->right) > 0) {
            root->right = rightRotate(root->right);
            return leftRotate(root);
        }

        return root;
    }

    bool search(AVLNode* node, int value) {
        if (!node) return false;
        if (value < node->value) return search(node->left, value);
        else if (value > node->value) return search(node->right, value);
        return true; // value found
    }

public:
    AVLTree() : root(nullptr) {}

    void insert(int value) {
        root = insert(root, value);
    }

    void deleteValue(int value) {
        root = deleteNode(root, value);
    }

    bool search(int value) {
        return search(root, value);
    }
};
```



### 使用示例



```cpp
int main() {
    AVLTree tree;
    tree.insert(10);
    tree.insert(20);
    tree.insert(30);
    cout << "Is 20 present? " << (tree.search(20) ? "Yes" : "No") << endl;
    tree.deleteValue(20);
    cout << "Is 20 present? " << (tree.search(20) ? "Yes" : "No") << endl;

    return 0;
}
```



这段代码定义了一个AVL树类，并提供了基本的插入、删除和查找功能。您可以在Visual Studio Code中运行并测试这段代码！