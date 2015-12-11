# 红黑树

讲红黑树就必须要先讲一下2-3树。因为红黑树实际上就是2-3树的一个简单实现。

2-3查找树[详细资料](http://www.cnblogs.com/yangecnu/p/Introduce-2-3-Search-Tree.html)  
红黑树资料：参见《算法》

### 2-3查找树定义：
* 一棵2-3查找树或者是一棵空树，或者由以下节点组成：
* 2-节点：含有一个key和两个孩子。左孩子都小于key，右孩子都大于key。
* 3-节点：含有两个key1、key2，左边的孩子小于key1，中间的孩子在key1和key2中间，右边的孩子大于key2.

***2-3查找树都是完美二叉树。***

### 2-3查找树插入元素：
* 向一个2-节点中插入元素：直接将2-节点变为三节点即可。
* 向一个父节点为2-节点的3-节点插入元素：可以先将该3-节点变为4-节点，然后将中间的key拿出来给父亲，使得父节点变为3-节点；而原来的三节点变为了两个2-节点。
* 向一个父节点为3-节点的3-节点插入元素：可以先将该3-节点变为4-节点，然后将中间的key拿出来给父亲，使得父节点变为4-节点；而原来的三节点变为了两个2-节点。然后父节点还需要接续向上调整，直到父亲节点为一个2-节点为止。或者最终到了根节点，根节点分裂的时候，会使得树高加1.

2-3查找树的概念比较简单易懂，而且树是一个完美二叉树。但是2-3查找树需要维护两种不同的节点，实现起来比较麻烦，而且情况也比较多。所以用红黑树来实现2-3查找树最好不过。

把2-3查找树中的3-节点变为两个节点，其中key1变为key2的左孩子。而且该链接是红色的。链接可以直接放到节点中。
可以证明，红黑树就是一个2-3查找树。

### 红黑树中插入元素
* 找到插入位置后插入一个红节点
* 然后需要按顺序做三种调整：
* 1，如果左孩子为黑右孩子为红，需要做一个左旋；
* 2，如果左孩子为红，左孩子的左孩子也为红，需要做一个右旋；
* 3，如果左右孩子都为红，则将两个孩子都变为黑，本身节点变为红。

红黑树的删除节点更加复杂。现在暂时先不弄了，回头要是有时间或者是有需要，再来搞明白吧。



红黑树部分代码：

    #include <stdio.h>
    #include <stdlib.h>
    
    typedef enum {
        RED,
        BLACK
    } COLOR;
    
    typedef struct node{
        int key, value;
        struct node *left, *right, *father;
        COLOR color;
    } Node;
    
    Node *create (int key, int value) {
        Node *ret = malloc(sizeof(Node));
        ret->key = key;
        ret->value = value;
        ret->color = RED;
        ret->left = ret->right = NULL;
    
        return ret;
    }
    
    int isRed(Node *node) {
        if (node == NULL) {
            return 0;
        }
        return node->color == RED;
    }
    
    Node *rotate_right(Node *root) {
        Node *ret = root->left;
        root->left = ret->right;
        ret->right = root;
    
        ret->color = root->color;
        root->color = RED;
    
        return ret;
    }
    
    Node *rotate_left(Node *root) {
        Node *ret = root->right;
        root->right = ret->left;
        ret->left = root;
    
        COLOR tmp = root->color;
        root->color = RED;
        ret->color = tmp;
    
        return ret;
    }
    
    void flip_color(Node *root) {
        root->left->color = BLACK;
        root->right->color = BLACK;
        root->color = RED;
    }
    
    Node * insert(Node *root, int key, int value) {
        if (root == NULL) {
            return create(key, value);
        }
    
        if (root->key > key) {
            root->left = insert(root->left, key, value);
        } else if (root->key < key) {
            root->right = insert(root->right, key, value);
        } else {
            root->value = value;
        }
    
        if (isRed(root->right) && !isRed(root->left)) {
            root = rotate_left(root);
        }
        if (isRed(root->left) && isRed(root->left->left)) {
            root = rotate_right(root);
        }
        if (isRed(root->left) && isRed(root->right)) {
            flip_color(root);
        }
    
        return root;
    }
    
    Node *find(Node *root, int key) {
        if (root == NULL) return NULL;
    
        if (root->key == key) return root;
        if (root->key > key) return find(root->left, key);
        else return find(root->right, key);
    }
    
    void print_tree(Node *root) {
        if (root == NULL) return;
        print_tree(root->left);
        printf("key = %d value = %d color = %d\n", root->key, root->value, root->color);
        print_tree(root->right);
    }
    
    int main() {
        Node *root = NULL;
    
        int k, v;
        while (scanf("%d%d", &k, &v) != EOF) {
            root = insert(root, k, v);
        }
    
        print_tree(root);
    
        while (scanf("%d", &k) != EOF) {
            Node *ret = find(root, k);
            if (ret == NULL) {
                printf("not exist\n");
            } else {
                printf("value = %d\n", root->value);
            }
        }
    
        return 0;
    }
