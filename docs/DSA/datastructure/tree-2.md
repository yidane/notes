数据结构中有很多树的结构，其中包括二叉树、二叉搜索树、2-3 树、红黑树等等。本文中对数据结构中常见的几种树的概念和用途进行了汇总，不求严格精准，但求简单易懂。

# **1\. 二叉树**

二叉树是数据结构中一种重要的数据结构，也是树表家族最为基础的结构。

**二叉树的定义：**二叉树的每个结点至多只有二棵子树 (不存在度大于 2 的结点)，二叉树的子树有左右之分，次序不能颠倒。二叉树的第 i 层至多有 2<sup>i-1</sup> 个结点；深度为 k 的二叉树至多有 2<sup>k-1</sup> 个结点；对任何一棵二叉树 T，如果其终端结点数为 n<sub>0</sub>，度为 2 的结点数为 n<sub>2</sub>，则 n<sub>0</sub>=n<sub>2</sub>+1。

**二叉树的示例**：

[![/assets/404.png](/assets/28995513.jpeg)](http://jbcdn2.b0.upaiyun.com/2017/07/bc42d6b7087d92bf57edac612206395f.jpg)

**满二叉树和完全二叉树：**

满二叉树：除最后一层无任何子节点外，每一层上的所有结点都有两个子结点。也可以这样理解，除叶子结点外的所有结点均有两个子结点。节点数达到最大值，所有叶子结点必须在同一层上。

满二叉树的性质：

1) 一颗树深度为 h，最大层数为 k，深度与最大层数相同，k=h;

1) 叶子数为 2<sup>h</sup>;

2) 第 k 层的结点数是：2<sup>k-1</sup>;

3) 总结点数是：2<sup>k-1</sup>，且总节点数一定是奇数。

完全二叉树：若设二叉树的深度为 h，除第 h 层外，其它各层 (1～(h-1) 层) 的结点数都达到最大个数，第 h 层所有的结点都连续集中在最左边，这就是完全二叉树。

**注：**完全二叉树是效率很高的数据结构，堆是一种完全二叉树或者近似完全二叉树，所以效率极高，像十分常用的排序算法、Dijkstra 算法、Prim 算法等都要用堆才能优化，二叉排序树的效率也要借助平衡性来提高，而平衡性基于完全二叉树。

[![/assets/404.png](/assets/28995514.png)](http://jbcdn2.b0.upaiyun.com/2017/07/a5952ec741b60202c7b377bfb8e8f368.png)

**二叉树的性质**：

1) 在非空二叉树中，第 i 层的结点总数不超过 2<sup>i-1</sup>, i>=1;

2) 深度为 h 的二叉树最多有 2<sup>h</sup>-1 个结点 (h>=1)，最少有 h 个结点;

3) 对于任意一棵二叉树，如果其叶结点数为 N<sub>0</sub>，而度数为 2 的结点总数为 N<sub>2</sub>，则 N<sub>0</sub>=N<sub>2</sub>+1;

4) 具有 n 个结点的完全二叉树的深度为 log<sub>2</sub>(n+1);

5) 有 N 个结点的完全二叉树各结点如果用顺序方式存储，则结点之间有如下关系：

若 I 为结点编号则 如果 I>1，则其父结点的编号为 I/2；

如果 2I<=N，则其左儿子（即左子树的根结点）的编号为 2I；若 2I>N，则无左儿子；

如果 2I+1<=N，则其右儿子的结点编号为 2I+1；若 2I+1>N，则无右儿子。

6) 给定 N 个节点，能构成 h(N) 种不同的二叉树，其中 h(N) 为卡特兰数的第 N 项，h(n)=C(2*n, n)/(n+1)。

7) 设有 i 个枝点，I 为所有枝点的道路长度总和，J 为叶的道路长度总和 J=I+2<sup>i</sup>。

# 2\. 二叉查找树

**二叉查找树定义**：又称为是二叉排序树（Binary Sort Tree）或二叉搜索树。二叉排序树或者是一棵空树，或者是具有下列性质的二叉树：

1) 若左子树不空，则左子树上所有结点的值均小于它的根结点的值；

2) 若右子树不空，则右子树上所有结点的值均大于或等于它的根结点的值；

3) 左、右子树也分别为二叉排序树；

4) 没有键值相等的节点。

**二叉查找树的性质：****对二叉查找树进行中序遍历，即可得到有序的数列。**

**二叉查找树的时间复杂度：**它和二分查找一样，插入和查找的时间复杂度均为 O(logn)，但是在最坏的情况下仍然会有 O(n) 的时间复杂度。原因在于插入和删除元素的时候，树没有保持平衡（比如，我们查找上图（b）中的 “93”，我们需要进行 n 次查找操作）。我们追求的是在最坏的情况下仍然有较好的时间复杂度，这就是平衡查找树设计的初衷。****

****二叉查找树的高度决定了二叉查找树的查找效率。****

**二叉查找树的插入过程如下：**

1) 若当前的二叉查找树为空，则插入的元素为根节点;

2) 若插入的元素值小于根节点值，则将元素插入到左子树中;

3) 若插入的元素值不小于根节点值，则将元素插入到右子树中。

**二叉查找树的删除，分三种情况进行处理：**

1) p 为叶子节点，直接删除该节点，再修改其父节点的指针（注意分是根节点和不是根节点），如图 a;

2) p 为单支节点（即只有左子树或右子树）。让 p 的子树与 p 的父亲节点相连，删除 p 即可（注意分是根节点和不是根节点），如图 b;

3) p 的左子树和右子树均不空。找到 p 的后继 y，因为 y 一定没有左子树，所以可以删除 y，并让 y 的父亲节点成为 y 的右子树的父亲节点，并用 y 的值代替 p 的值；或者方法二是找到 p 的前驱 x，x 一定没有右子树，所以可以删除 x，并让 x 的父亲节点成为 y 的左子树的父亲节点。如图 c。

[![/assets/404.png](/assets/28995522.png)](http://jbcdn2.b0.upaiyun.com/2017/07/2361a6d889a036b92d6fa44c8984d27e.png)

[![/assets/404.png](/assets/28995543.png)](http://jbcdn2.b0.upaiyun.com/2017/07/05b0ae29caf51cf3aeb26e30ee6996ec.png)[![/assets/404.png](/assets/28995544.png)](http://jbcdn2.b0.upaiyun.com/2017/07/2c51f44d9bfb87f38f9bf040552b6b31.png)

二叉树相关实现源码：

插入操作：

```c
struct node { int val; pnode lchild; pnode rchild; }; pnode BT = NULL; //递归方法插入节点 pnode insert(pnode root, int x) { pnode p = (pnode)malloc(LEN); p->val = x; p->lchild = NULL; p->rchild = NULL; if(root == NULL){ root = p; } else if(x < root->val){ root->lchild = insert(root->lchild, x); } else{ root->rchild = insert(root->rchild, x); } return root; } //非递归方法插入节点 void insert_BST(pnode q, int x) { pnode p = (pnode)malloc(LEN); p->val = x; p->lchild = NULL; p->rchild = NULL; if(q == NULL){ BT = p; return ; } while(q->lchild != p && q->rchild != p){ if(x < q->val){ if(q->lchild){ q = q->lchild; } else{ q->lchild = p; } } else{ if(q->rchild){ q = q->rchild; } else{ q->rchild = p; } } } return; }
```

| 123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960 | struct node{    int val;    pnode lchild;    pnode rchild;}; pnode BT = NULL;  // 递归方法插入节点 pnode insert(pnode root, int x){    pnode p = (pnode)malloc(LEN);    p->val = x;    p->lchild = NULL;    p->rchild = NULL;    if(root == NULL){        root = p;        }        else if(x < root->val){        root->lchild = insert(root->lchild, x);        }    else{        root->rchild = insert(root->rchild, x);        }    return root;} // 非递归方法插入节点 void insert_BST(pnode q, int x){    pnode p = (pnode)malloc(LEN);    p->val = x;    p->lchild = NULL;    p->rchild = NULL;    if(q == NULL){        BT = p;        return ;        }            while(q->lchild != p && q->rchild != p){        if(x < q->val){            if(q->lchild){                q = q->lchild;                }                else{                q->lchild = p;            }                }            else{            if(q->rchild){                q = q->rchild;                }                else{                q->rchild = p;                }        }    }    return;} |

删除操作：

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 13px !important; line-height: 15px !important; z-index: 0; opacity: 0;">bool delete_BST(pnode p, int x) //返回一个标志，表示是否找到被删元素 { bool find = false; pnode q; p = BT; while(p && !find){ //寻找被删元素 if(x == p->val){ //找到被删元素 find = true; } else if(x < p->val){ //沿左子树找 q = p; p = p->lchild; } else{ //沿右子树找 q = p; p = p->rchild; } } if(p == NULL){ //没找到 cout << "没有找到" << x << endl; } if(p->lchild == NULL && p->rchild == NULL){ //p为叶子节点 if(p == BT){ //p为根节点 BT = NULL; } else if(q->lchild == p){ q->lchild = NULL; } else{ q->rchild = NULL; } free(p); //释放节点p } else if(p->lchild == NULL || p->rchild == NULL){ //p为单支子树 if(p == BT){ //p为根节点 if(p->lchild == NULL){ BT = p->rchild; } else{ BT = p->lchild; } } else{ if(q->lchild == p && p->lchild){ //p是q的左子树且p有左子树 q->lchild = p->lchild; //将p的左子树链接到q的左指针上 } else if(q->lchild == p && p->rchild){ q->lchild = p->rchild; } else if(q->rchild == p && p->lchild){ q->rchild = p->lchild; } else{ q->rchild = p->rchild; } } free(p); } else{ //p的左右子树均不为空 pnode t = p; pnode s = p->lchild; //从p的左子节点开始 while(s->rchild){ //找到p的前驱，即p左子树中值最大的节点 t = s; s = s->rchild; } p->val = s->val; //把节点s的值赋给p if(t == p){ p->lchild = s->lchild; } else{ t->rchild = s->lchild; } free(s); } return find; }</textarea>

| 1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677 | bool delete_BST(pnode p, int x) // 返回一个标志，表示是否找到被删元素 {    bool find = false;    pnode q;    p = BT;    while(p && !find){  // 寻找被删元素         if(x == p->val){  // 找到被删元素             find = true;            }            else if(x < p->val){ // 沿左子树找             q = p;            p = p->lchild;            }        else{   // 沿右子树找             q = p;            p = p->rchild;            }    }    if(p == NULL){   // 没找到         cout << "没有找到" << x << endl;        }        if(p->lchild == NULL && p->rchild == NULL){  //p 为叶子节点         if(p == BT){  //p 为根节点             BT = NULL;            }        else if(q->lchild == p){               q->lchild = NULL;        }                else{            q->rchild = NULL;            }        free(p);  // 释放节点 p     }    else if(p->lchild == NULL || p->rchild == NULL){ //p 为单支子树         if(p == BT){  //p 为根节点             if(p->lchild == NULL){                BT = p->rchild;                }                else{                BT = p->lchild;                }        }            else{            if(q->lchild == p && p->lchild){ //p 是 q 的左子树且 p 有左子树                 q->lchild = p->lchild;    // 将 p 的左子树链接到 q 的左指针上             }                else if(q->lchild == p && p->rchild){                q->lchild = p->rchild;                }            else if(q->rchild == p && p->lchild){                q->rchild = p->lchild;                }            else{                q->rchild = p->rchild;            }        }        free(p);    }    else{ //p 的左右子树均不为空         pnode t = p;        pnode s = p->lchild;  // 从 p 的左子节点开始         while(s->rchild){  // 找到 p 的前驱，即 p 左子树中值最大的节点             t = s;               s = s->rchild;            }        p->val = s->val;   // 把节点 s 的值赋给 p         if(t == p){            p->lchild = s->lchild;            }            else{            t->rchild = s->lchild;            }        free(s);     }    return find;} |

查找操作：

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 13px !important; line-height: 15px !important; z-index: 0; opacity: 0;">pnode search_BST(pnode p, int x) { bool solve = false; while(p && !solve){ if(x == p->val){ solve = true; } else if(x < p->val){ p = p->lchild; } else{ p = p->rchild; } } if(p == NULL){ cout << "没有找到" << x << endl; } return p; }</textarea>

| 12345678910111213141516171819 | pnode search_BST(pnode p, int x){    bool solve = false;    while(p && !solve){        if(x == p->val){            solve = true;            }            else if(x < p->val){            p = p->lchild;            }        else{            p = p->rchild;            }    }    if(p == NULL){        cout << "没有找到" << x << endl;        }     return p;} |

# 3\. 平衡二叉树

我们知道，对于一般的二叉搜索树（Binary Search Tree），其期望高度（即为一棵平衡树时）为 log<sub>2</sub>n，其各操作的时间复杂度 O(log<sub>2</sub>n) 同时也由此而决定。但是，在某些极端的情况下（如在插入的序列是有序的时），二叉搜索树将退化成近似链或链，此时，其操作的时间复杂度将退化成线性的，即 O(n)。我们可以通过随机化建立二叉搜索树来尽量的避免这种情况，但是在进行了多次的操作之后，由于在删除时，我们总是选择将待删除节点的后继代替它本身，这样就会造成总是右边的节点数目减少，以至于树向左偏沉。这同时也会造成树的平衡性受到破坏，提高它的操作的时间复杂度。于是就有了我们下边介绍的平衡二叉树。

**平衡二叉树定义：**平衡二叉树（Balanced Binary Tree）又被称为 AVL 树（有别于 AVL 算法），且具有以下性质：它是一 棵空树或它的左右两个子树的高度差的绝对值不超过 1，并且左右两个子树都是一棵平衡二叉树。平衡二叉树的常用算法有红黑树、AVL 树等。在平衡二叉搜索树中，我们可以看到，其高度一般都良好地维持在 O(log<sub>2</sub>n)，大大降低了操作的时间复杂度。

最小二叉平衡树的节点的公式如下：

F(n)=F(n-1)+F(n-2)+1

这个类似于一个递归的数列，可以参考 Fibonacci 数列，1 是根节点，F(n-1) 是左子树的节点数量，F(n-2) 是右子树的节点数量。

## 3.1 平衡查找树之 AVL 树

有关 AVL 树的具体实现，可以参考 [C 小加的博客《一步一步写平衡二叉树（AVL）》](http://www.cppblog.com/cxiaojia/archive/2012/08/20/187776.html)。

**AVL 树定义：**AVL 树是最先发明的自平衡二叉查找树。AVL 树得名于它的发明者 G.M. Adelson-Velsky 和 E.M. Landis，他们在 1962 年的论文 “An algorithm for the organization of information” 中发表了它。在 AVL 中任何节点的两个儿子子树的高度最大差别为 1，所以它也被称为高度平衡树，n 个结点的 AVL 树最大深度约 1.44log<sub>2</sub>n。查找、插入和删除在平均和最坏情况下都是 O(logn)。增加和删除可能需要通过一次或多次树旋转来重新平衡这个树。**这个方案很好的解决了二叉查找树退化成链表的问题，把插入，查找，删除的时间复杂度最好情况和最坏情况都维持在 O(logN)。但是频繁旋转会使插入和删除牺牲掉 O(logN) 左右的时间，不过相对二叉查找树来说，时间上稳定了很多。**

**AVL 树的自平衡操作——旋转：**

AVL 树最关键的也是最难的一步操作就是**旋转**。旋转主要是为了实现 AVL 树在实施了插入和删除操作以后，树重新回到平衡的方法。下面我们重点研究一下 AVL 树的旋转。

对于一个平衡的节点，由于任意节点最多有两个儿子，因此高度不平衡时，此节点的两颗子树的高度差 2\. 容易看出，这种不平衡出现在下面四种情况：

[![/assets/404.png](/assets/28995545.jpeg)](http://jbcdn2.b0.upaiyun.com/2017/07/bbc1d195a4adf1a9cc10bb85b0f1c2c6.jpg)

1) 6 节点的左子树 3 节点高度比右子树 7 节点大 2，左子树 3 节点的左子树 1 节点高度大于右子树 4 节点，这种情况成为左左。

2) 6 节点的左子树 2 节点高度比右子树 7 节点大 2，左子树 2 节点的左子树 1 节点高度小于右子树 4 节点，这种情况成为左右。

3) 2 节点的左子树 1 节点高度比右子树 5 节点小 2，右子树 5 节点的左子树 3 节点高度大于右子树 6 节点，这种情况成为右左。

4) 2 节点的左子树 1 节点高度比右子树 4 节点小 2，右子树 4 节点的左子树 3 节点高度小于右子树 6 节点，这种情况成为右右。

从图 2 中可以可以看出，1 和 4 两种情况是对称的，这两种情况的旋转算法是一致的，只需要经过一次旋转就可以达到目标，我们称之为单旋转。2 和 3 两种情况也是对称的，这两种情况的旋转算法也是一致的，需要进行两次旋转，我们称之为双旋转。

**单旋转**

单旋转是针对于左左和右右这两种情况的解决方案，这两种情况是对称的，只要解决了左左这种情况，右右就很好办了。图 3 是左左情况的解决方案，节点 k2 不满足平衡特性，因为它的左子树 k1 比右子树 Z 深 2 层，而且 k1 子树中，更深的一层的是 k1 的左子树 X 子树，所以属于左左情况。

[![/assets/404.png](/assets/28995546.jpeg)](http://jbcdn2.b0.upaiyun.com/2017/07/61ee0c83b3783cb05eac2ed3528eae3b.jpg)

为使树恢复平衡，我们把 k2 变成这棵树的根节点，因为 k2 大于 k1，把 k2 置于 k1 的右子树上，而原本在 k1 右子树的 Y 大于 k1，小于 k2，就把 Y 置于 k2 的左子树上，这样既满足了二叉查找树的性质，又满足了平衡二叉树的性质。

这样的操作只需要一部分指针改变，结果我们得到另外一颗二叉查找树，它是一棵 AVL 树，因为 X 向上一移动了一层，Y 还停留在原来的层面上，Z 向下移动了一层。整棵树的新高度和之前没有在左子树上插入的高度相同，插入操作使得 X 高度长高了。因此，由于这颗子树高度没有变化，所以通往根节点的路径就不需要继续旋转了。

**双旋转**

对于左右和右左这两种情况，单旋转不能使它达到一个平衡状态，要经过两次旋转。双旋转是针对于这两种情况的解决方案，同样的，这样两种情况也是对称的，只要解决了左右这种情况，右左就很好办了。图 4 是左右情况的解决方案，节点 k3 不满足平衡特性，因为它的左子树 k1 比右子树 Z 深 2 层，而且 k1 子树中，更深的一层的是 k1 的右子树 k2 子树，所以属于左右情况。

[![/assets/404.png](/assets/28995547.jpeg)](http://jbcdn2.b0.upaiyun.com/2017/07/2ad609dadbff982036a8e32c792436d7.jpg)

为使树恢复平衡，我们需要进行两步，第一步，把 k1 作为根，进行一次右右旋转，旋转之后就变成了左左情况，所以第二步再进行一次左左旋转，最后得到了一棵以 k2 为根的平衡二叉树。

**AVL 树实现源码：**

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 13px !important; line-height: 15px !important; z-index: 0; opacity: 0;">//AVL树节点信息 template<class T> class TreeNode { public: TreeNode():lson(NULL),rson(NULL),freq(1),hgt(0){} T data;//值 int hgt;//高度 unsigned int freq;//频率 TreeNode* lson;//指向左儿子的地址 TreeNode* rson;//指向右儿子的地址 }; //AVL树类的属性和方法声明 template<class T> class AVLTree { private: TreeNode<T>* root;//根节点 void insertpri(TreeNode<T>* &node,T x);//插入 TreeNode<T>* findpri(TreeNode<T>* node,T x);//查找 void insubtree(TreeNode<T>* node);//中序遍历 void Deletepri(TreeNode<T>* &node,T x);//删除 int height(TreeNode<T>* node);//求树的高度 void SingRotateLeft(TreeNode<T>* &k2);//左左情况下的旋转 void SingRotateRight(TreeNode<T>* &k2);//右右情况下的旋转 void DoubleRotateLR(TreeNode<T>* &k3);//左右情况下的旋转 void DoubleRotateRL(TreeNode<T>* &k3);//右左情况下的旋转 int Max(int cmpa,int cmpb);//求最大值 public: AVLTree():root(NULL){} void insert(T x);//插入接口 TreeNode<T>* find(T x);//查找接口 void Delete(T x);//删除接口 void traversal();//遍历接口 }; //计算节点的高度 template<class T> int AVLTree<T>::height(TreeNode<T>* node) { if(node!=NULL) return node->hgt; return -1; } //求最大值 template<class T> int AVLTree<T>::Max(int cmpa,int cmpb) { return cmpa>cmpb?cmpa:cmpb; } //左左情况下的旋转 template<class T> void AVLTree<T>::SingRotateLeft(TreeNode<T>* &k2) { TreeNode<T>* k1; k1=k2->lson; k2->lson=k1->rson; k1->rson=k2; k2->hgt=Max(height(k2->lson),height(k2->rson))+1; k1->hgt=Max(height(k1->lson),k2->hgt)+1; } //右右情况下的旋转 template<class T> void AVLTree<T>::SingRotateRight(TreeNode<T>* &k2) { TreeNode<T>* k1; k1=k2->rson; k2->rson=k1->lson; k1->lson=k2; k2->hgt=Max(height(k2->lson),height(k2->rson))+1; k1->hgt=Max(height(k1->rson),k2->hgt)+1; } //左右情况的旋转 template<class T> void AVLTree<T>::DoubleRotateLR(TreeNode<T>* &k3) { SingRotateRight(k3->lson); SingRotateLeft(k3); } //右左情况的旋转 template<class T> void AVLTree<T>::DoubleRotateRL(TreeNode<T>* &k3) { SingRotateLeft(k3->rson); SingRotateRight(k3); } //插入 template<class T> void AVLTree<T>::insertpri(TreeNode<T>* &node,T x) { if(node==NULL)//如果节点为空,就在此节点处加入x信息 { node=new TreeNode<T>(); node->data=x; return; } if(node->data>x)//如果x小于节点的值,就继续在节点的左子树中插入x { insertpri(node->lson,x); if(2==height(node->lson)-height(node->rson)) if(x<node->lson->data) SingRotateLeft(node); else DoubleRotateLR(node); } else if(node->data<x)//如果x大于节点的值,就继续在节点的右子树中插入x { insertpri(node->rson,x); if(2==height(node->rson)-height(node->lson))//如果高度之差为2的话就失去了平衡,需要旋转 if(x>node->rson->data) SingRotateRight(node); else DoubleRotateRL(node); } else ++(node->freq);//如果相等,就把频率加1 node->hgt=Max(height(node->lson),height(node->rson)); } //插入接口 template<class T> void AVLTree<T>::insert(T x) { insertpri(root,x); } //查找 template<class T> TreeNode<T>* AVLTree<T>::findpri(TreeNode<T>* node,T x) { if(node==NULL)//如果节点为空说明没找到,返回NULL { return NULL; } if(node->data>x)//如果x小于节点的值,就继续在节点的左子树中查找x { return findpri(node->lson,x); } else if(node->data<x)//如果x大于节点的值,就继续在节点的左子树中查找x { return findpri(node->rson,x); } else return node;//如果相等,就找到了此节点 } //查找接口 template<class T> TreeNode<T>* AVLTree<T>::find(T x) { return findpri(root,x); } //删除 template<class T> void AVLTree<T>::Deletepri(TreeNode<T>* &node,T x) { if(node==NULL) return ;//没有找到值是x的节点 if(x < node->data) { Deletepri(node->lson,x);//如果x小于节点的值,就继续在节点的左子树中删除x if(2==height(node->rson)-height(node->lson)) if(node->rson->lson!=NULL&&(height(node->rson->lson)>height(node->rson->rson)) ) DoubleRotateRL(node); else SingRotateRight(node); } else if(x > node->data) { Deletepri(node->rson,x);//如果x大于节点的值,就继续在节点的右子树中删除x if(2==height(node->lson)-height(node->rson)) if(node->lson->rson!=NULL&& (height(node->lson->rson)>height(node->lson->lson) )) DoubleRotateLR(node); else SingRotateLeft(node); } else//如果相等,此节点就是要删除的节点 { if(node->lson&&node->rson)//此节点有两个儿子 { TreeNode<T>* temp=node->rson;//temp指向节点的右儿子 while(temp->lson!=NULL) temp=temp->lson;//找到右子树中值最小的节点 //把右子树中最小节点的值赋值给本节点 node->data=temp->data; node->freq=temp->freq; Deletepri(node->rson,temp->data);//删除右子树中最小值的节点 if(2==height(node->lson)-height(node->rson)) { if(node->lson->rson!=NULL&& (height(node->lson->rson)>height(node->lson->lson) )) DoubleRotateLR(node); else SingRotateLeft(node); } } else//此节点有1个或0个儿子 { TreeNode<T>* temp=node; if(node->lson==NULL)//有右儿子或者没有儿子 node=node->rson; else if(node->rson==NULL)//有左儿子 node=node->lson; delete(temp); temp=NULL; } } if(node==NULL) return; node->hgt=Max(height(node->lson),height(node->rson))+1; return; } //删除接口 template<class T> void AVLTree<T>::Delete(T x) { Deletepri(root,x); } //中序遍历函数 template<class T> void AVLTree<T>::insubtree(TreeNode<T>* node) { if(node==NULL) return; insubtree(node->lson);//先遍历左子树 cout<<node->data<<" ";//输出根节点 insubtree(node->rson);//再遍历右子树 } //中序遍历接口 template<class T> void AVLTree<T>::traversal() { insubtree(root); }</textarea>

| 123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229 | //AVL 树节点信息template<class T>class TreeNode{    public:        TreeNode():lson(NULL),rson(NULL),freq(1),hgt(0){}        T data;// 值        int hgt;// 高度        unsigned int freq;// 频率        TreeNode* lson;// 指向左儿子的地址        TreeNode* rson;// 指向右儿子的地址};//AVL 树类的属性和方法声明template<class T>class AVLTree{    private:        TreeNode<T>* root;// 根节点        void insertpri(TreeNode<T>* &node,T x);// 插入        TreeNode<T>* findpri(TreeNode<T>* node,T x);// 查找        void insubtree(TreeNode<T>* node);// 中序遍历        void Deletepri(TreeNode<T>* &node,T x);// 删除        int height(TreeNode<T>* node);// 求树的高度        void SingRotateLeft(TreeNode<T>* &k2);// 左左情况下的旋转        void SingRotateRight(TreeNode<T>* &k2);// 右右情况下的旋转        void DoubleRotateLR(TreeNode<T>* &k3);// 左右情况下的旋转        void DoubleRotateRL(TreeNode<T>* &k3);// 右左情况下的旋转        int Max(int cmpa,int cmpb);// 求最大值     public:        AVLTree():root(NULL){}        void insert(T x);// 插入接口        TreeNode<T>* find(T x);// 查找接口        void Delete(T x);// 删除接口        void traversal();// 遍历接口 };// 计算节点的高度template<class T>int AVLTree<T>::height(TreeNode<T>* node){    if(node!=NULL)        return node->hgt;    return -1;}// 求最大值template<class T>int AVLTree<T>::Max(int cmpa,int cmpb){    return cmpa>cmpb?cmpa:cmpb;}// 左左情况下的旋转template<class T>void AVLTree<T>::SingRotateLeft(TreeNode<T>* &k2){    TreeNode<T>* k1;    k1=k2->lson;    k2->lson=k1->rson;    k1->rson=k2;     k2->hgt=Max(height(k2->lson),height(k2->rson))+1;    k1->hgt=Max(height(k1->lson),k2->hgt)+1;}// 右右情况下的旋转template<class T>void AVLTree<T>::SingRotateRight(TreeNode<T>* &k2){    TreeNode<T>* k1;    k1=k2->rson;    k2->rson=k1->lson;    k1->lson=k2;     k2->hgt=Max(height(k2->lson),height(k2->rson))+1;    k1->hgt=Max(height(k1->rson),k2->hgt)+1;}// 左右情况的旋转template<class T>void AVLTree<T>::DoubleRotateLR(TreeNode<T>* &k3){    SingRotateRight(k3->lson);    SingRotateLeft(k3);}// 右左情况的旋转template<class T>void AVLTree<T>::DoubleRotateRL(TreeNode<T>* &k3){    SingRotateLeft(k3->rson);    SingRotateRight(k3);}// 插入template<class T>void AVLTree<T>::insertpri(TreeNode<T>* &node,T x){    if(node==NULL)// 如果节点为空, 就在此节点处加入 x 信息    {        node=new TreeNode<T>();        node->data=x;        return;    }    if(node->data>x)// 如果 x 小于节点的值, 就继续在节点的左子树中插入 x    {        insertpri(node->lson,x);        if(2==height(node->lson)-height(node->rson))            if(x<node->lson->data)                SingRotateLeft(node);            else                DoubleRotateLR(node);    }    else if(node->data<x)// 如果 x 大于节点的值, 就继续在节点的右子树中插入 x    {        insertpri(node->rson,x);        if(2==height(node->rson)-height(node->lson))// 如果高度之差为 2 的话就失去了平衡, 需要旋转            if(x>node->rson->data)                SingRotateRight(node);            else                DoubleRotateRL(node);    }    else ++(node->freq);// 如果相等, 就把频率加 1    node->hgt=Max(height(node->lson),height(node->rson));}// 插入接口template<class T>void AVLTree<T>::insert(T x){    insertpri(root,x);}// 查找template<class T>TreeNode<T>* AVLTree<T>::findpri(TreeNode<T>* node,T x){    if(node==NULL)// 如果节点为空说明没找到, 返回 NULL    {        return NULL;    }    if(node->data>x)// 如果 x 小于节点的值, 就继续在节点的左子树中查找 x    {        return findpri(node->lson,x);    }    else if(node->data<x)// 如果 x 大于节点的值, 就继续在节点的左子树中查找 x    {        return findpri(node->rson,x);    }    else return node;// 如果相等, 就找到了此节点}// 查找接口template<class T>TreeNode<T>* AVLTree<T>::find(T x){    return findpri(root,x);}// 删除template<class T>void AVLTree<T>::Deletepri(TreeNode<T>* &node,T x){    if(node==NULL) return ;// 没有找到值是 x 的节点    if(x < node->data)    {         Deletepri(node->lson,x);// 如果 x 小于节点的值, 就继续在节点的左子树中删除 x         if(2==height(node->rson)-height(node->lson))            if(node->rson->lson!=NULL&&(height(node->rson->lson)>height(node->rson->rson)) )                DoubleRotateRL(node);            else                SingRotateRight(node);    }     else if(x > node->data)    {         Deletepri(node->rson,x);// 如果 x 大于节点的值, 就继续在节点的右子树中删除 x         if(2==height(node->lson)-height(node->rson))            if(node->lson->rson!=NULL&& (height(node->lson->rson)>height(node->lson->lson) ))                DoubleRotateLR(node);            else                SingRotateLeft(node);    }     else// 如果相等, 此节点就是要删除的节点    {        if(node->lson&&node->rson)// 此节点有两个儿子        {            TreeNode<T>* temp=node->rson;//temp 指向节点的右儿子            while(temp->lson!=NULL) temp=temp->lson;// 找到右子树中值最小的节点            // 把右子树中最小节点的值赋值给本节点            node->data=temp->data;            node->freq=temp->freq;            Deletepri(node->rson,temp->data);// 删除右子树中最小值的节点            if(2==height(node->lson)-height(node->rson))            {                if(node->lson->rson!=NULL&& (height(node->lson->rson)>height(node->lson->lson) ))                    DoubleRotateLR(node);                else                    SingRotateLeft(node);            }        }        else// 此节点有 1 个或 0 个儿子        {            TreeNode<T>* temp=node;            if(node->lson==NULL)// 有右儿子或者没有儿子            node=node->rson;            else if(node->rson==NULL)// 有左儿子            node=node->lson;            delete(temp);            temp=NULL;        }    }    if(node==NULL) return;    node->hgt=Max(height(node->lson),height(node->rson))+1;    return;}// 删除接口template<class T>void AVLTree<T>::Delete(T x){    Deletepri(root,x);}// 中序遍历函数template<class T>void AVLTree<T>::insubtree(TreeNode<T>* node){    if(node==NULL) return;    insubtree(node->lson);// 先遍历左子树    cout<<node->data<<" ";// 输出根节点    insubtree(node->rson);// 再遍历右子树}// 中序遍历接口template<class T>void AVLTree<T>::traversal(){    insubtree(root);} |

## 3.2 平衡二叉树之红黑树

**红黑树的定义：**红黑树是一种自平衡二叉查找树，是在计算机科学中用到的一种数据结构，典型的用途是实现关联数组。它是在 1972 年由鲁道夫 · 贝尔发明的，称之为” 对称二叉 B 树”，它现代的名字是在 Leo J. Guibas 和 Robert Sedgewick 于 1978 年写的一篇论文中获得的。**它是复杂的，但它的操作有着良好的最坏情况运行时间，并且在实践中是高效的: 它可以在 O(logn) 时间内做查找，插入和删除，这里的 n 是树中元素的数目。**

红黑树和 AVL 树一样都对插入时间、删除时间和查找时间提供了最好可能的最坏情况担保。这不只是使它们在时间敏感的应用如实时应用（real time application）中有价值，而且使它们有在提供最坏情况担保的其他数据结构中作为建造板块的价值；例如，在计算几何中使用的很多数据结构都可以基于红黑树。此外，红黑树还是 2-3-4 树的一种等同，它们的思想是一样的，只不过红黑树是 2-3-4 树用二叉树的形式表示的。

**红黑树的性质：**

红黑树是每个节点都带有颜色属性的二叉查找树，颜色为红色或黑色。在二叉查找树强制的一般要求以外，对于任何有效的红黑树我们增加了如下的额外要求:

性质 1\. 节点是红色或黑色。

性质 2\. 根是黑色。

性质 3\. 所有叶子都是黑色（叶子是 NIL 节点）。

性质 4\. 每个红色节点必须有两个黑色的子节点。(从每个叶子到根的所有路径上不能有两个连续的红色节点。)

性质 5\. 从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。

下面是一个具体的红黑树的图例：

[![/assets/404.png](/assets/28995548.png)](http://jbcdn2.b0.upaiyun.com/2017/07/9fd5e683147961431e0ecfcffbe5805b.png)

这些约束确保了红黑树的关键特性: 从根到叶子的最长的可能路径不多于最短的可能路径的两倍长。结果是这个树大致上是平衡的。因为操作比如插入、删除和查找某个值的最坏情况时间都要求与树的高度成比例，这个在高度上的理论上限允许红黑树在最坏情况下都是高效的，而不同于普通的二叉查找树。

要知道为什么这些性质确保了这个结果，注意到性质 4 导致了路径不能有两个毗连的红色节点就足够了。最短的可能路径都是黑色节点，最长的可能路径有交替的红色和黑色节点。因为根据性质 5 所有最长的路径都有相同数目的黑色节点，这就表明了没有路径能多于任何其他路径的两倍长。

以下内容整理自 [wiki 百科之红黑树](https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91)。

**红黑树的自平衡操作：**

因为每一个红黑树也是一个特化的二叉查找树，因此红黑树上的只读操作与普通二叉查找树上的只读操作相同。然而，在红黑树上进行插入操作和删除操作会导致不再符合红黑树的性质。恢复红黑树的性质需要少量 (O(logn)) 的颜色变更 (实际是非常快速的) 和不超过三次树旋转(对于插入操作是两次)。虽然插入和删除很复杂，但操作时间仍可以保持为 O(logn) 次。

**我们首先以二叉查找树的方法增加节点并标记它为红色。如果设为黑色，就会导致根到叶子的路径上有一条路上，多一个额外的黑节点，这个是很难调整的（违背性质 5）。**但是设为红色节点后，可能会导致出现两个连续红色节点的冲突，那么可以通过颜色调换（color flips）和树旋转来调整。下面要进行什么操作取决于其他临近节点的颜色。同人类的家族树中一样，我们将使用术语叔父节点来指一个节点的父节点的兄弟节点。注意:

*   性质 1 和性质 3 总是保持着。
*   性质 4 只在增加红色节点、重绘黑色节点为红色，或做旋转时受到威胁。
*   性质 5 只在增加黑色节点、重绘红色节点为黑色，或做旋转时受到威胁。

**插入操作：**

假设，将要插入的节点标为 **N**，N 的父节点标为 **P**，N 的祖父节点标为 **G**，N 的叔父节点标为 **U**。在图中展示的任何颜色要么是由它所处情形这些所作的假定，要么是假定所暗含的。

**情形 1: **该树为空树，直接插入根结点的位置，违反性质 1，把节点颜色有红改为黑即可。

**情形 2:** 插入节点 N 的父节点 P 为黑色，不违反任何性质，无需做任何修改。在这种情形下，树仍是有效的。性质 5 也未受到威胁，尽管新节点 N 有两个黑色叶子子节点；但由于新节点 N 是红色，通过它的每个子节点的路径就都有同通过它所取代的黑色的叶子的路径同样数目的黑色节点，所以依然满足这个性质。

注： 情形 1 很简单，情形 2 中 P 为黑色，一切安然无事，但 P 为红就不一样了，下边是 P 为红的各种情况，也是真正难懂的地方。

**情形 3: **如果父节点 P 和叔父节点 U 二者都是红色，(此时新插入节点 N 做为 P 的左子节点或右子节点都属于情形 3, 这里右图仅显示 N 做为 P 左子的情形) 则我们可以将它们两个重绘为黑色并重绘祖父节点 G 为红色 (用来保持性质 4)。现在我们的新节点 N 有了一个黑色的父节点 P。因为通过父节点 P 或叔父节点 U 的任何路径都必定通过祖父节点 G，在这些路径上的黑节点数目没有改变。但是，红色的祖父节点 G 的父节点也有可能是红色的，这就违反了性质 4。为了解决这个问题，我们在祖父节点 G 上递归地进行上述情形的整个过程（把 G 当成是新加入的节点进行各种情形的检查）。比如，G 为根节点，那我们就直接将 G 变为黑色（情形 1）；如果 G 不是根节点，而它的父节点为黑色，那符合所有的性质，直接插入即可（情形 2）；如果 G 不是根节点，而它的父节点为红色，则递归上述过程（情形 3）。

[![/assets/404.png](/assets/28995549.png)](http://jbcdn2.b0.upaiyun.com/2017/07/dc3747336812e9055f958c56ade89eb8.png)

**情形 4:** 父节点 P 是红色而叔父节点 U 是黑色或缺少，新节点 N 是其父节点的左子节点，而父节点 P 又是其父节点 G 的左子节点。在这种情形下，我们进行针对祖父节点 G 的一次右旋转; 在旋转产生的树中，以前的父节点 P 现在是新节点 N 和以前的祖父节点 G 的父节点。我们知道以前的祖父节点 G 是黑色，否则父节点 P 就不可能是红色 (如果 P 和 G 都是红色就违反了性质 4，所以 G 必须是黑色)。我们切换以前的父节点 P 和祖父节点 G 的颜色，结果的树满足性质 4。性质 5 也仍然保持满足，因为通过这三个节点中任何一个的所有路径以前都通过祖父节点 G，现在它们都通过以前的父节点 P。在各自的情形下，这都是三个节点中唯一的黑色节点。

[![/assets/404.png](/assets/28995550.png)](http://jbcdn2.b0.upaiyun.com/2017/07/4986c959603100e41cba2669d9e3ce0c.png)

**情形 5:** 父节点 P 是红色而叔父节点 U 是黑色或缺少，并且新节点 N 是其父节点 P 的右子节点而父节点 P 又是其父节点的左子节点。在这种情形下，我们进行一次左旋转调换新节点和其父节点的角色; 接着，我们按**情形 4** 处理以前的父节点 P 以解决仍然失效的性质 4。注意这个改变会导致某些路径通过它们以前不通过的新节点 N（比如图中 1 号叶子节点）或不通过节点 P（比如图中 3 号叶子节点），但由于这两个节点都是红色的，所以性质 5 仍有效。

[![/assets/404.png](/assets/28995551.png)](http://jbcdn2.b0.upaiyun.com/2017/07/9cfbf62dfdefe5891347de95a976758f.png)

**注: 插入实际上是原地算法，因为上述所有调用都使用了尾部递归。**

**删除操作：**

**如果需要删除的节点有两个儿子，那么问题可以被转化成删除另一个只有一个儿子的节点的问题**。对于二叉查找树，在删除带有两个非叶子儿子的节点的时候，我们找到要么在它的左子树中的最大元素、要么在它的右子树中的最小元素，并把它的值转移到要删除的节点中。我们接着删除我们从中复制出值的那个节点，它必定有少于两个非叶子的儿子。因为只是复制了一个值，不违反任何性质，这就把问题简化为如何删除最多有一个儿子的节点的问题。它不关心这个节点是最初要删除的节点还是我们从中复制出值的那个节点。

**我们只需要讨论删除只有一个儿子的节点** (如果它两个儿子都为空，即均为叶子，我们任意将其中一个看作它的儿子)。如果我们删除一个红色节点（此时该节点的儿子将都为叶子节点），它的父亲和儿子一定是黑色的。所以我们可以简单的用它的黑色儿子替换它，并不会破坏性质 3 和性质 4。通过被删除节点的所有路径只是少了一个红色节点，这样可以继续保证性质 5。另一种简单情况是在被删除节点是黑色而它的儿子是红色的时候。如果只是去除这个黑色节点，用它的红色儿子顶替上来的话，会破坏性质 5，但是如果我们重绘它的儿子为黑色，则曾经通过它的所有路径将通过它的黑色儿子，这样可以继续保持性质 5。

**需要进一步讨论的是在要删除的节点和它的儿子二者都是黑色的时候**，这是一种复杂的情况。我们首先把要删除的节点替换为它的儿子。出于方便，称呼这个儿子为 **N**(在新的位置上)，称呼它的兄弟 (它父亲的另一个儿子) 为 **S**。在下面的示意图中，我们还是使用 **P** 称呼 N 的父亲，**S<sub>L</sub>** 称呼 S 的左儿子，**S<sub>R</sub>** 称呼 S 的右儿子。

如果 N 和它初始的父亲是黑色，则删除它的父亲导致通过 N 的路径都比不通过它的路径少了一个黑色节点。因为这违反了性质 5，树需要被重新平衡。有几种情形需要考虑:

**情形 1:** N 是新的根。在这种情形下，我们就做完了。我们从所有路径去除了一个黑色节点，而新根是黑色的，所以性质都保持着。

**注意**: 在情形 2、5 和 6 下，我们假定 N 是它父亲的左儿子。如果它是右儿子，则在这些情形下的左和右应当对调。

**情形 2:** S 是红色。在这种情形下我们在 N 的父亲上做左旋转，把红色兄弟转换成 N 的祖父，我们接着对调 N 的父亲和祖父的颜色。完成这两个操作后，尽管所有路径上黑色节点的数目没有改变，但现在 N 有了一个黑色的兄弟和一个红色的父亲（它的新兄弟是黑色因为它是红色 S 的一个儿子），所以我们可以接下去按**情形 4**、**情形 5** 或**情形 6** 来处理。

[![/assets/404.png](/assets/28995552.png)](http://jbcdn2.b0.upaiyun.com/2017/07/ce86b0e24e64c35067f5d087dc4c90ae.png)

**情形 3:** N 的父亲、S 和 S 的儿子都是黑色的。在这种情形下，我们简单的重绘 S 为红色。结果是通过 S 的所有路径，它们就是以前_不_通过 N 的那些路径，都少了一个黑色节点。因为删除 N 的初始的父亲使通过 N 的所有路径少了一个黑色节点，这使事情都平衡了起来。但是，通过 P 的所有路径现在比不通过 P 的路径少了一个黑色节点，所以仍然违反性质 5。要修正这个问题，我们要从**情形 1** 开始，在 P 上做重新平衡处理。

[![/assets/404.png](/assets/28995553.png)](http://jbcdn2.b0.upaiyun.com/2017/07/30759b4136b3cd56ba31eb8d06a434d2.png)

**情形 4:** S 和 S 的儿子都是黑色，但是 N 的父亲是红色。在这种情形下，我们简单的交换 N 的兄弟和父亲的颜色。这不影响不通过 N 的路径的黑色节点的数目，但是它在通过 N 的路径上对黑色节点数目增加了一，添补了在这些路径上删除的黑色节点。

[![/assets/404.png](/assets/28995554.png)](http://jbcdn2.b0.upaiyun.com/2017/07/61bda9c9a89a3f9ea5f01ed990070a39.png)

**情形 5:** S 是黑色，S 的左儿子是红色，S 的右儿子是黑色，而 N 是它父亲的左儿子。在这种情形下我们在 S 上做右旋转，这样 S 的左儿子成为 S 的父亲和 N 的新兄弟。我们接着交换 S 和它的新父亲的颜色。所有路径仍有同样数目的黑色节点，但是现在 N 有了一个黑色兄弟，他的右儿子是红色的，所以我们进入了**情形 6**。N 和它的父亲都不受这个变换的影响。

[![/assets/404.png](/assets/28995558.png)](http://jbcdn2.b0.upaiyun.com/2017/07/9f411d70951995007ad2a59fb747d160.png)

**情形 6:** S 是黑色，S 的右儿子是红色，而 N 是它父亲的左儿子。在这种情形下我们在 N 的父亲上做左旋转，这样 S 成为 N 的父亲（P）和 S 的右儿子的父亲。我们接着交换 N 的父亲和 S 的颜色，并使 S 的右儿子为黑色。子树在它的根上的仍是同样的颜色，所以性质 3 没有被违反。但是，N 现在增加了一个黑色祖先: 要么 N 的父亲变成黑色，要么它是黑色而 S 被增加为一个黑色祖父。所以，通过 N 的路径都增加了一个黑色节点。

此时，如果一个路径不通过 N，则有两种可能性:

*   它通过 N 的新兄弟。那么它以前和现在都必定通过 S 和 N 的父亲，而它们只是交换了颜色。所以路径保持了同样数目的黑色节点。
*   它通过 N 的新叔父，S 的右儿子。那么它以前通过 S、S 的父亲和 S 的右儿子，但是现在只通过 S，它被假定为它以前的父亲的颜色，和 S 的右儿子，它被从红色改变为黑色。合成效果是这个路径通过了同样数目的黑色节点。

在任何情况下，在这些路径上的黑色节点数目都没有改变。所以我们恢复了性质 4。在示意图中的白色节点可以是红色或黑色，但是在变换前后都必须指定相同的颜色。

[![/assets/404.png](/assets/28995559.png)](http://jbcdn2.b0.upaiyun.com/2017/07/c0ea423eef5e175e0ba92e43ad17270d.png)

**红黑树实现源码：**

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 13px !important; line-height: 15px !important; z-index: 0; opacity: 0;">#define BLACK 1 #define RED 0 using namespace std; class bst { private: struct Node { int value; bool color; Node *leftTree, *rightTree, *parent; Node() { color = RED; leftTree = NULL; rightTree = NULL; parent = NULL; value = 0; } Node* grandparent() { if (parent == NULL) { return NULL; } return parent->parent; } Node* uncle() { if (grandparent() == NULL) { return NULL; } if (parent == grandparent()->rightTree) return grandparent()->leftTree; else return grandparent()->rightTree; } Node* sibling() { if (parent->leftTree == this) return parent->rightTree; else return parent->leftTree; } }; void rotate_right(Node *p) { Node *gp = p->grandparent(); Node *fa = p->parent; Node *y = p->rightTree; fa->leftTree = y; if (y != NIL) y->parent = fa; p->rightTree = fa; fa->parent = p; if (root == fa) root = p; p->parent = gp; if (gp != NULL) { if (gp->leftTree == fa) gp->leftTree = p; else gp->rightTree = p; } } void rotate_left(Node *p) { if (p->parent == NULL) { root = p; return; } Node *gp = p->grandparent(); Node *fa = p->parent; Node *y = p->leftTree; fa->rightTree = y; if (y != NIL) y->parent = fa; p->leftTree = fa; fa->parent = p; if (root == fa) root = p; p->parent = gp; if (gp != NULL) { if (gp->leftTree == fa) gp->leftTree = p; else gp->rightTree = p; } } void inorder(Node *p) { if (p == NIL) return; if (p->leftTree) inorder(p->leftTree); cout << p->value << " "; if (p->rightTree) inorder(p->rightTree); } string outputColor(bool color) { return color ? "BLACK" : "RED"; } Node* getSmallestChild(Node *p) { if (p->leftTree == NIL) return p; return getSmallestChild(p->leftTree); } bool delete_child(Node *p, int data) { if (p->value > data) { if (p->leftTree == NIL) { return false; } return delete_child(p->leftTree, data); } else if (p->value < data) { if (p->rightTree == NIL) { return false; } return delete_child(p->rightTree, data); } else if (p->value == data) { if (p->rightTree == NIL) { delete_one_child(p); return true; } Node *smallest = getSmallestChild(p->rightTree); swap(p->value, smallest->value); delete_one_child(smallest); return true; } } void delete_one_child(Node *p) { Node *child = p->leftTree == NIL ? p->rightTree : p->leftTree; if (p->parent == NULL && p->leftTree == NIL && p->rightTree == NIL) { p = NULL; root = p; return; } if (p->parent == NULL) { delete p; child->parent = NULL; root = child; root->color = BLACK; return; } if (p->parent->leftTree == p) { p->parent->leftTree = child; } else { p->parent->rightTree = child; } child->parent = p->parent; if (p->color == BLACK) { if (child->color == RED) { child->color = BLACK; } else delete_case(child); } delete p; } void delete_case(Node *p) { if (p->parent == NULL) { p->color = BLACK; return; } if (p->sibling()->color == RED) { p->parent->color = RED; p->sibling()->color = BLACK; if (p == p->parent->leftTree) rotate_left(p->sibling()); else rotate_right(p->sibling()); } if (p->parent->color == BLACK && p->sibling()->color == BLACK && p->sibling()->leftTree->color == BLACK && p->sibling()->rightTree->color == BLACK) { p->sibling()->color = RED; delete_case(p->parent); } else if (p->parent->color == RED && p->sibling()->color == BLACK && p->sibling()->leftTree->color == BLACK && p->sibling()->rightTree->color == BLACK) { p->sibling()->color = RED; p->parent->color = BLACK; } else { if (p->sibling()->color == BLACK) { if (p == p->parent->leftTree && p->sibling()->leftTree->color == RED && p->sibling()->rightTree->color == BLACK) { p->sibling()->color = RED; p->sibling()->leftTree->color = BLACK; rotate_right(p->sibling()->leftTree); } else if (p == p->parent->rightTree && p->sibling()->leftTree->color == BLACK && p->sibling()->rightTree->color == RED) { p->sibling()->color = RED; p->sibling()->rightTree->color = BLACK; rotate_left(p->sibling()->rightTree); } } p->sibling()->color = p->parent->color; p->parent->color = BLACK; if (p == p->parent->leftTree) { p->sibling()->rightTree->color = BLACK; rotate_left(p->sibling()); } else { p->sibling()->leftTree->color = BLACK; rotate_right(p->sibling()); } } } void insert(Node *p, int data) { if (p->value >= data) { if (p->leftTree != NIL) insert(p->leftTree, data); else { Node *tmp = new Node(); tmp->value = data; tmp->leftTree = tmp->rightTree = NIL; tmp->parent = p; p->leftTree = tmp; insert_case(tmp); } } else { if (p->rightTree != NIL) insert(p->rightTree, data); else { Node *tmp = new Node(); tmp->value = data; tmp->leftTree = tmp->rightTree = NIL; tmp->parent = p; p->rightTree = tmp; insert_case(tmp); } } } void insert_case(Node *p) { if (p->parent == NULL) { root = p; p->color = BLACK; return; } if (p->parent->color == RED) { if (p->uncle()->color == RED) { p->parent->color = p->uncle()->color = BLACK; p->grandparent()->color = RED; insert_case(p->grandparent()); } else { if (p->parent->rightTree == p && p->grandparent()->leftTree == p->parent) { rotate_left(p); rotate_right(p); p->color = BLACK; p->leftTree->color = p->rightTree->color = RED; } else if (p->parent->leftTree == p && p->grandparent()->rightTree == p->parent) { rotate_right(p); rotate_left(p); p->color = BLACK; p->leftTree->color = p->rightTree->color = RED; } else if (p->parent->leftTree == p && p->grandparent()->leftTree == p->parent) { p->parent->color = BLACK; p->grandparent()->color = RED; rotate_right(p->parent); } else if (p->parent->rightTree == p && p->grandparent()->rightTree == p->parent) { p->parent->color = BLACK; p->grandparent()->color = RED; rotate_left(p->parent); } } } } void DeleteTree(Node *p) { if (!p || p == NIL) { return; } DeleteTree(p->leftTree); DeleteTree(p->rightTree); delete p; } public: bst() { NIL = new Node(); NIL->color = BLACK; root = NULL; } ~bst() { if (root) DeleteTree(root); delete NIL; } void inorder() { if (root == NULL) return; inorder(root); cout << endl; } void insert(int x) { if (root == NULL) { root = new Node(); root->color = BLACK; root->leftTree = root->rightTree = NIL; root->value = x; } else { insert(root, x); } } bool delete_value(int data) { return delete_child(root, data); } private: Node *root, *NIL; };</textarea>

| 123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256257258259260261262263264265266267268269270271272273274275276277278279280281282283284285286287288289290291292293294295296297298299300301302303304305306307308309310311312313314315316317318319320321322323324325326327328329330331332333 | #define BLACK 1#define RED 0 using namespace std; class bst {private:     struct Node {        int value;        bool color;        Node *leftTree, *rightTree, *parent;         Node() {            color = RED;            leftTree = NULL;            rightTree = NULL;            parent = NULL;            value = 0;        }         Node* grandparent() {            if (parent == NULL) {                return NULL;            }            return parent->parent;        }         Node* uncle() {            if (grandparent() == NULL) {                return NULL;            }            if (parent == grandparent()->rightTree)                return grandparent()->leftTree;            else                return grandparent()->rightTree;        }         Node* sibling() {            if (parent->leftTree == this)                return parent->rightTree;            else                return parent->leftTree;        }    };     void rotate_right(Node *p) {        Node *gp = p->grandparent();        Node *fa = p->parent;        Node *y = p->rightTree;         fa->leftTree = y;         if (y != NIL)            y->parent = fa;        p->rightTree = fa;        fa->parent = p;         if (root == fa)            root = p;        p->parent = gp;         if (gp != NULL) {            if (gp->leftTree == fa)                gp->leftTree = p;            else                gp->rightTree = p;        }     }     void rotate_left(Node *p) {        if (p->parent == NULL) {            root = p;            return;        }        Node *gp = p->grandparent();        Node *fa = p->parent;        Node *y = p->leftTree;         fa->rightTree = y;         if (y != NIL)            y->parent = fa;        p->leftTree = fa;        fa->parent = p;         if (root == fa)            root = p;        p->parent = gp;         if (gp != NULL) {            if (gp->leftTree == fa)                gp->leftTree = p;            else                gp->rightTree = p;        }    }     void inorder(Node *p) {        if (p == NIL)            return;         if (p->leftTree)            inorder(p->leftTree);         cout << p->value << " ";                        if (p->rightTree)            inorder(p->rightTree);    }     string outputColor(bool color) {        return color ? "BLACK" : "RED";    }     Node* getSmallestChild(Node *p) {        if (p->leftTree == NIL)            return p;        return getSmallestChild(p->leftTree);    }     bool delete_child(Node *p, int data) {        if (p->value > data) {            if (p->leftTree == NIL) {                return false;            }            return delete_child(p->leftTree, data);        } else if (p->value < data) {            if (p->rightTree == NIL) {                return false;            }            return delete_child(p->rightTree, data);        } else if (p->value == data) {            if (p->rightTree == NIL) {                delete_one_child(p);                return true;            }            Node *smallest = getSmallestChild(p->rightTree);            swap(p->value, smallest->value);            delete_one_child(smallest);             return true;        }    }     void delete_one_child(Node *p) {        Node *child = p->leftTree == NIL ? p->rightTree : p->leftTree;        if (p->parent == NULL && p->leftTree == NIL && p->rightTree == NIL) {            p = NULL;            root = p;            return;        }                if (p->parent == NULL) {            delete  p;            child->parent = NULL;            root = child;            root->color = BLACK;            return;        }                if (p->parent->leftTree == p) {            p->parent->leftTree = child;        } else {            p->parent->rightTree = child;        }        child->parent = p->parent;         if (p->color == BLACK) {            if (child->color == RED) {                child->color = BLACK;            } else                delete_case(child);        }         delete p;    }     void delete_case(Node *p) {        if (p->parent == NULL) {            p->color = BLACK;            return;        }        if (p->sibling()->color == RED) {            p->parent->color = RED;            p->sibling()->color = BLACK;            if (p == p->parent->leftTree)                rotate_left(p->sibling());            else                rotate_right(p->sibling());        }        if (p->parent->color == BLACK && p->sibling()->color == BLACK                && p->sibling()->leftTree->color == BLACK && p->sibling()->rightTree->color == BLACK) {            p->sibling()->color = RED;            delete_case(p->parent);        } else if (p->parent->color == RED && p->sibling()->color == BLACK                && p->sibling()->leftTree->color == BLACK && p->sibling()->rightTree->color == BLACK) {            p->sibling()->color = RED;            p->parent->color = BLACK;        } else {            if (p->sibling()->color == BLACK) {                if (p == p->parent->leftTree && p->sibling()->leftTree->color == RED                        && p->sibling()->rightTree->color == BLACK) {                    p->sibling()->color = RED;                    p->sibling()->leftTree->color = BLACK;                    rotate_right(p->sibling()->leftTree);                } else if (p == p->parent->rightTree && p->sibling()->leftTree->color == BLACK                        && p->sibling()->rightTree->color == RED) {                    p->sibling()->color = RED;                    p->sibling()->rightTree->color = BLACK;                    rotate_left(p->sibling()->rightTree);                }            }            p->sibling()->color = p->parent->color;            p->parent->color = BLACK;            if (p == p->parent->leftTree) {                p->sibling()->rightTree->color = BLACK;                rotate_left(p->sibling());            } else {                p->sibling()->leftTree->color = BLACK;                rotate_right(p->sibling());            }        }    }     void insert(Node *p, int data) {        if (p->value >= data) {            if (p->leftTree != NIL)                insert(p->leftTree, data);            else {                Node *tmp = new Node();                tmp->value = data;                tmp->leftTree = tmp->rightTree = NIL;                tmp->parent = p;                p->leftTree = tmp;                insert_case(tmp);            }        } else {            if (p->rightTree != NIL)                insert(p->rightTree, data);            else {                Node *tmp = new Node();                tmp->value = data;                tmp->leftTree = tmp->rightTree = NIL;                tmp->parent = p;                p->rightTree = tmp;                insert_case(tmp);            }        }    }     void insert_case(Node *p) {        if (p->parent == NULL) {            root = p;            p->color = BLACK;            return;        }        if (p->parent->color == RED) {            if (p->uncle()->color == RED) {                p->parent->color = p->uncle()->color = BLACK;                p->grandparent()->color = RED;                insert_case(p->grandparent());            } else {                if (p->parent->rightTree == p && p->grandparent()->leftTree == p->parent) {                    rotate_left(p);                    rotate_right(p);                    p->color = BLACK;                    p->leftTree->color = p->rightTree->color = RED;                } else if (p->parent->leftTree == p && p->grandparent()->rightTree == p->parent) {                    rotate_right(p);                    rotate_left(p);                    p->color = BLACK;                    p->leftTree->color = p->rightTree->color = RED;                } else if (p->parent->leftTree == p && p->grandparent()->leftTree == p->parent) {                    p->parent->color = BLACK;                    p->grandparent()->color = RED;                    rotate_right(p->parent);                } else if (p->parent->rightTree == p && p->grandparent()->rightTree == p->parent) {                    p->parent->color = BLACK;                    p->grandparent()->color = RED;                    rotate_left(p->parent);                }            }        }    }     void DeleteTree(Node *p) {        if (!p || p == NIL) {            return;        }        DeleteTree(p->leftTree);        DeleteTree(p->rightTree);        delete p;    }public:     bst() {        NIL = new Node();        NIL->color = BLACK;        root = NULL;    }     ~bst() {        if (root)            DeleteTree(root);        delete NIL;    }     void inorder() {        if (root == NULL)            return;        inorder(root);        cout << endl;    }     void insert(int x) {        if (root == NULL) {            root = new Node();            root->color = BLACK;            root->leftTree = root->rightTree = NIL;            root->value = x;        } else {            insert(root, x);        }    }     bool delete_value(int data) {        return delete_child(root, data);    }private:    Node *root, *NIL;}; |

# 4\. B 树

B 树也是一种用于查找的平衡树，但是它不是二叉树。

**B 树的定义：**B 树（B-tree）是一种树状数据结构，能够用来存储排序后的数据。这种数据结构能够让查找数据、循序存取、插入数据及删除的动作，都在对数时间内完成。B 树，概括来说是一个一般化的二叉查找树，可以拥有多于 2 个子节点。与自平衡二叉查找树不同，B - 树为系统最优化大块数据的读和写操作。B-tree 算法减少定位记录时所经历的中间过程，从而加快存取速度。这种数据结构常被应用在数据库和文件系统的实作上。

在 B 树中查找给定关键字的方法是，首先把根结点取来，在根结点所包含的关键字 K1,…,Kn 查找给定的关键字（可用顺序查找或二分查找法），若找到等于给定值的关键字，则查找成功；否则，一定可以确定要查找的关键字在 Ki 与 Ki+1 之间，Pi 为指向子树根节点的指针，此时取指针 Pi 所指的结点继续查找，直至找到，或指针 Pi 为空时查找失败。

B 树作为一种多路搜索树（并不是二叉的）：

1) 定义任意非叶子结点最多只有 M 个儿子；且 M>2；

2) 根结点的儿子数为 [2, M]；

3) 除根结点以外的非叶子结点的儿子数为 [M/2, M]；

4) 每个结点存放至少 M/2-1（取上整）和至多 M-1 个关键字；（至少 2 个关键字）

5) 非叶子结点的关键字个数 = 指向儿子的指针个数 - 1；

6) 非叶子结点的关键字：K[1], K[2], …, K[M-1]；且 K[i] < K[i+1]；

7) 非叶子结点的指针：P[1], P[2], …, P[M]；其中 P[1]指向关键字小于 K[1]的子树，P[M]指向关键字大于 K[M-1]的子树，其它 P[i]指向关键字属于 (K[i-1], K[i]) 的子树；

8) 所有叶子结点位于同一层；

如下图为一个 M=3 的 B 树示例：

[![/assets/404.png](/assets/28995560.jpeg)](http://jbcdn2.b0.upaiyun.com/2017/07/0178191b698ab75a98fa1d0bb03cc51f.jpg)

B 树创建的示意图：

![/assets/404.png](/assets/28995563.jpeg)

# 5\. B + 树

B + 树是 B 树的变体，也是一种多路搜索树：

1) 其定义基本与 B - 树相同，除了：

2) 非叶子结点的子树指针与关键字个数相同；

3) 非叶子结点的子树指针 P[i]，指向关键字值属于 [K[i], K[i+1]) 的子树（B - 树是开区间）；

4) 为所有叶子结点增加一个链指针；

5) 所有关键字都在叶子结点出现；

下图为 M=3 的 B + 树的示意图：

[![/assets/404.png](/assets/28995565.jpeg)](http://jbcdn2.b0.upaiyun.com/2017/07/0972ef809f286cc29cd2d94687b2ef2d.jpg)

B + 树的搜索与 B 树也基本相同，区别是 B + 树只有达到叶子结点才命中（B 树可以在非叶子结点命中），其性能也等价于在关键字全集做一次二分查找；

**B + 的性质：**

1\. 所有关键字都出现在叶子结点的链表中（稠密索引），且链表中的关键字恰好是有序的；

2\. 不可能在非叶子结点命中；

3\. 非叶子结点相当于是叶子结点的索引（稀疏索引），叶子结点相当于是存储（关键字）数据的数据层；

4\. 更适合文件索引系统。

下面为一个 B + 树创建的示意图：

![/assets/404.png](/assets/28995568.jpeg)

# 6\. B * 树

B * 树是 B + 树的变体，在 B + 树的非根和非叶子结点再增加指向兄弟的指针，将结点的最低利用率从 1/2 提高到 2/3。

B * 树如下图所示：

[![/assets/404.png](/assets/28995569.jpeg)](http://jbcdn2.b0.upaiyun.com/2017/07/eb5835f421e029240105ccb8e80279ee.jpg)

B * 树定义了非叶子结点关键字个数至少为 (2/3)*M，即块的最低使用率为 2/3（代替 B + 树的 1/2）；

B + 树的分裂：当一个结点满时，分配一个新的结点，并将原结点中 1/2 的数据复制到新结点，最后在父结点中增加新结点的指针；B + 树的分裂只影响原结点和父结点，而不会影响兄弟结点，所以它不需要指向兄弟的指针；

B * 树的分裂：当一个结点满时，如果它的下一个兄弟结点未满，那么将一部分数据移到兄弟结点中，再在原结点插入关键字，最后修改父结点中兄弟结点的关键字（因为兄弟结点的关键字范围改变了）；如果兄弟也满了，则在原结点与兄弟结点之间增加新结点，并各复制 1/3 的数据到新结点，最后在父结点增加新结点的指针；

所以，B * 树分配新结点的概率比 B + 树要低，空间使用率更高。

# 7\. Trie 树

Tire 树称为字典树，又称单词查找树，Trie 树，是一种树形结构，是一种哈希树的变种。典型应用是用于统计，排序和保存大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。**它的优点是：利用字符串的公共前缀来减少查询时间，最大限度地减少无谓的字符串比较，查询效率比哈希树高。**

**Tire 树的三个基本性质：** 1) 根节点不包含字符，除根节点外每一个节点都只包含一个字符； 2) 从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串； 3) 每个节点的所有子节点包含的字符都不相同。

**Tire 树的应用：**

1) 串的快速检索

给出 N 个单词组成的熟词表，以及一篇全用小写英文书写的文章，请你按最早出现的顺序写出所有不在熟词表中的生词。在这道题中，我们可以用数组枚举，用哈希，用字典树，先把熟词建一棵树，然后读入文章进行比较，这种方法效率是比较高的。

2) “串” 排序

给定 N 个互不相同的仅由一个单词构成的英文名，让你将他们按字典序从小到大输出。用字典树进行排序，采用数组的方式创建字典树，这棵树的每个结点的所有儿子很显然地按照其字母大小排序。对这棵树进行先序遍历即可。

3) 最长公共前缀

对所有串建立字典树，对于两个串的最长公共前缀的长度即他们所在的结点的公共祖先个数，于是，问题就转化为求公共祖先的问题。