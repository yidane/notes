# 数据结构（十三）——树

## 一、树的简介

### 1、树的简介

树是一种非线性的数据结构，是由 n（n >=0）个结点组成的有限集合。
如果 n==0，树为空树。
如果 n>0，
树有一个特定的结点，根结点
根结点只有直接后继，没有直接前驱。
除根结点以外的其他结点划分为 m（m>=0）个互不相交的有限集合，T0，T1，T2，...，Tm-1，每个结合是一棵树，称为根结点的子树。
树的示例如下：
![/assets/404.png](/assets/28995515.png)

### 2、树的度

树的结点包含一个数据和多个指向子树的分支
结点拥有的子树的数量为结点的度，度为 0 的结点是叶结点，度不为 0 的结点为分支结点，树的度定义为树的所有结点中度的最大值。
![/assets/404.png](/assets/28995516.png)

### 3、树的前驱和后继

结点的直接后继称为结点的孩子，结点称为孩子的双亲。
结点的孩子的孩子称为结点的孙子，结点称为子孙的祖先。
同一个双亲的孩子之间互称兄弟。
![/assets/404.png](/assets/28995517.png)

### 4、树中结点的层次

![/assets/404.png](/assets/28995518.png)
树中根结点为第 1 层，根结点的孩子为第 2 层，依次类推。
树中结点的最大层次称为树的深度或高度

### 5、树的有序性

如果树中结点的各子树从左向右是有序的，子树间不能互换位置，则称该树为有序树，否则为无序树。
![/assets/404.png](/assets/28995519.png)

### 6、森林

森林是由 n 棵互不相交的树组成的集合。
三棵树组成的森林如下：
![/assets/404.png](/assets/28995520.png)

## 二、树的抽象实现

### 1、树的抽象实现

树在程序中表现为一种特殊的数据类型。
树的抽象实现如下：

```
template <typename T>
  class Tree:public Object
  {
  protected:
    TreeNode<T>* m_root;//根结点
  public:
    Tree(){m_root = NULL;}
    //插入结点
    virtual bool insert(TreeNode<T>* node) = 0;
    virtual bool insert(const T& value, TreeNode<T>* parent) = 0;
    //删除结点
    virtual SharedPointer< Tree<T> > remove(const T& value) = 0;
    virtual SharedPointer< Tree<T> > remove(TreeNode<T>* node) = 0;
    //查找结点
    virtual TreeNode<T>* find(const T& value)const = 0;
    virtual TreeNode<T>* find(TreeNode<T>* node)const = 0;
    //根结点访问函数
    virtual TreeNode<T>* root()const = 0;
    //树的度访问函数
    virtual int degree()const = 0;
    //树的高度访问函数
    virtual int height()const = 0;
    //树的结点数目访问函数
    virtual int count()const = 0;
    //清空树
    virtual void clear() = 0;
  };
```

### 2、树结点的抽象实现

树中的结点表现为一种特殊的数据类型。
结点的抽象实现如下：

```
template <typename T>
  class TreeNode:public Object
  {
  public:
    T value;
    TreeNode<T>* parent;
    TreeNode()
    {
      parent = NULL;
    }
    virtual ~TreeNode() = 0;
  };
  template <typename T>
  TreeNode<T>::~TreeNode()
  {
  }
```

## 三、树的操作

### 1、树和树结点的存储结构实现

GTreeNode 能够包含任意多个指向后继结点的指针。

```
template <typename T>
  class GTreeNode:public TreeNode<T>
  {
  protected:
    LinkedList<GTreeNode<T>*> m_children;
  };
GTree为通用树结构，每个结点可以存在多个后继结点。
  template <typename T>
  class GTree:public Tree<T>
  {
  public:
  };
```

![/assets/404.png](/assets/28995525.png)
每个树结点包含指向前驱结点的指针，优点在于可以将非线性的树转化为线性的结构。
![/assets/404.png](/assets/28995526.png)

### 2、树中结点的查找

A、基于数据元素值的查找
定义在任意一个结点为根结点的树中查找指定数据元素值所在的结点的函数。
![/assets/404.png](/assets/28995527.png)

```
GTreeNode<T>* find(GTreeNode<T>* node, const T& value)const
    {
      GTreeNode<T>* ret = NULL;
      if(node != NULL)
      {
          //如果根结点的就是目标结点
          if(node->value == value)
          {
              ret =  node;
          }
          else
          {
              //遍历根节点的子结点
              for(node->m_children.move(0); !node->m_children.end() && (ret == NULL); node->m_children.next())
              {
                  //对每个子子结点进行查找
                  ret = find(node->m_children.current(), value);
              }
          }
      }
      return ret;
    }

    //查找结点
    virtual GTreeNode<T>* find(const T& value)const
    {
      return find(root(), value);
    }
```

B、基于树中结点的查找
定义在任意一个结点为根结点的树中查找指定结点的函数
![/assets/404.png](/assets/28995528.png)

```
 GTreeNode<T>* find(GTreeNode<T>* node, GTreeNode<T>* obj)const
    {
      GTreeNode<T>* ret = NULL;
      //根结点为目标结点
      if(node == obj)
      {
          ret =  node;
      }
      else
      {
          if(node != NULL)
          {
              //遍历子结点
              for(node->m_children.move(0); !node->m_children.end() && (ret == NULL); node->m_children.next())
              {
                  ret = find(node->m_children.current(), obj);
              }
          }
      }
      return ret;
    }

    virtual GTreeNode<T>* find(TreeNode<T>* node)const
    {
      return find(root(), dynamic_cast<GTreeNode<T>*>(node));
    }
```

### 3、树中结点的插入

树是非线性的数据结构，无法采用下标的方式定位数据元素，由于每个结点的都有一个唯一的前驱结点（父结点），因此必须找到前驱结点才能找到插入的位置。
A、插入结点
插入结点的流程
![/assets/404.png](/assets/28995529.png)

```
bool insert(TreeNode<T>* node)
    {
      bool ret = true;
      if(node != NULL)
      {
          //树为空，插入结点为根结点
          if(this->m_root == NULL)
          {
              node->parent = NULL;
              this->m_root = node;
          }
          else
          {
              //找到插入结点的父结点
              GTreeNode<T>* np = find(node->parent);
              if(np != NULL)
              {
                  GTreeNode<T>* n = dynamic_cast<GTreeNode<T>*>(node);
                  //如果子结点中无该结点，插入结点
                  if(np->m_children.find(n) < 0)
                  {
                      ret = np->m_children.insert(n);
                  }
              }
              else
              {
                  THROW_EXCEPTION(InvalidOperationException, "Invalid node...");
              }
          }
      }
      else
      {
          THROW_EXCEPTION(InvalidParameterException, "Parameter is invalid...");
      }
      return ret;
    }
```

B、插入数据元素
插入数据元素到树中的流程如下：
![/assets/404.png](/assets/28995531.png)

```
bool insert(const T& value, TreeNode<T>* parent)
    {
      bool ret = true;
      GTreeNode<T>* node = GTreeNode<T>::NewNode();
      if(node != NULL)
      {
          node->value = value;
          node->parent = parent;
          insert(node);
      }
      else
      {
          THROW_EXCEPTION(NoEnoughMemoryException, "No enough memory...");
      }
      return ret;
    }
```

### 4、树中结点的清除

树的清空操作需要将树中所有的结点清除，并释放分配在堆中的结点。
定义清除树中每个结点的函数
![/assets/404.png](/assets/28995532.png)
如果树中的结点可能分配在栈上和堆上，需要将堆空间的结点进行释放。
根据内存地址不能准确判断结点所在的存储空间，清除时存储在栈上的结点并不需要释放，只需要释放存储在堆空间的结点。
使用工厂模式对分配在堆空间的结点进行定制。
A、GTreeNode 类中增加保护的堆空间标识成员 m_flag
B、重载 GTreeNode 的 operrator new 操作符，声明为保护
C、提供工厂方法 NewNode（），在工厂方法中创建堆空间的结点，并将 m_flag 标识置为 true。

```
template <typename T>
  class GTreeNode:public TreeNode<T>
  {
  protected:
    bool m_flag;//堆空间标识
    //重载new操作符，声明为保护
    void* operator new(unsigned int size)throw()
    {
      return Object::operator new(size);
    }

  public:
    LinkedList<GTreeNode<T>*> m_children;
    GTreeNode()
    {
      //栈上分配的空间标识为false
      m_flag = false;
    }
    //工厂方法，创建堆空间的结点
    static GTreeNode<T>* NewNode()
    {
      GTreeNode<T>* ret = new GTreeNode<T>();
      if(ret != NULL)
      {
          //堆空间的结点标识为true
          ret->m_flag = true;
      }
      return ret;
    }
    //堆空间结点标识访问函数
    bool flag()const
    {
      return m_flag;
    }
  };
结点的释放：
    void free(GTreeNode<T>* node)
    {
      if(node != NULL)
      {
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              free(node->m_children.current());
          }
          //如果结点存储在堆空间
          if(node->flag())
             delete node;//释放
      }
    }
清空树：
    void clear()
    {
        free(root());
        this->m_root = NULL;
    }
```

### 5、树中结点的删除

删除函数的设计要点：
将被删除结点的子树进行删除；
删除函数返回一颗堆空间的树；
具体返回值为指向树的智能指针；
具体的结点删除功能函数如下：
将 node 为根结点的子树从原来的树中删除，ret 作为子树返回
![/assets/404.png](/assets/28995561.png)

```
void remove(GTreeNode<T>* node, GTree<T>*& ret)
    {
      ret = new GTree<T>();
      if(ret != NULL)
      {
          //如果删除的结点是根结点
          if(root() == node)
          {
              this->m_root = NULL;
          }
          else
          {
              //获取删除结点的父结点的子结点链表
              LinkedList<GTreeNode<T>*>& child = dynamic_cast<GTreeNode<T>*>(node->parent)->m_children;
              //从链表中删除结点
              child.remove(child.find(node));
              //结点的父结点置NULL
              node->parent = NULL;
          }
          //将删除结点赋值给创建的树ret的根结点
          ret->m_root = node;
      }
      else
      {
          THROW_EXCEPTION(NoEnoughMemoryException, "No enough memory...");
      }
    }
```

A、基于删除数据元素值删除结点

```
 SharedPointer<Tree<T>> remove(const T& value)
    {
      GTree<T>* ret = NULL;
      //找到结点
      GTreeNode<T>* node = find(value);
      if(node != NULL)
      {
          remove(node, ret);
      }
      else
      {
          THROW_EXCEPTION(InvalidParameterException, "Parameter invalid...");
      }
      return ret;
    }
```

B、基于结点删除

```
 SharedPointer<Tree<T>> remove(TreeNode<T>* node)
    {
      GTree<T>* ret = NULL;
      node = find(node);
      if(node != NULL)
      {
          remove(dynamic_cast<GTreeNode<T>*>(node), ret);
      }
      else
      {
          THROW_EXCEPTION(InvalidParameterException, "Parameter invalid...");
      }
      return ret;
    }
```

### 6、树中结点的属性操作

A、树中结点的数量
以 node 为结点的子树的结点数量，递归模型如下：
![/assets/404.png](/assets/28995562.png)

```
int count(GTreeNode<T>* node) const
    {
      int ret = 0;
      if(node != NULL)
      {
          ret = 1;//根结点
          //遍历根节点的子结点
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              ret += count(node->m_children.current());
          }
      }
      return ret;
    }
    //树的结点数目访问函数
    int count()const
    {
        count(root());
    }
```

B、树的度
结点 node 为根的树的度的功能函数的递归模型如下：
![/assets/404.png](/assets/28995564.png)

```
int degree(GTreeNode<T>* node) const
    {
      int ret = 0;
      if(node != NULL)
      {
          //结点的子结点的数量
          ret = node->m_children.length();
          //遍历子结点
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              int d = degree(node->m_children.current());
              if(ret < d)
              {
                  ret = d;
              }
          }
      }
      return ret;
    }

    //树的度访问函数
    int degree()const
    {
        return degree(root());
    }
```

C、树的高度
结点 node 为根的树的高度的功能函数的递归模型如下：
![/assets/404.png](/assets/28995566.png)

```
int height(GTreeNode<T>* node)const
    {
      int ret = 0;
      if(node != NULL)
      {
          //遍历子结点
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              //当前结点的高度
              int h = height(node->m_children.current());
              if(ret < h)
              {
                  ret = h;
              }
          }
          ret = ret + 1;
      }
      return ret;
    }
    //树的高度访问函数
    int height()const
    {
        height(root());
    }
```

### 7、树中结点的遍历

树是一种非线性的数据结构，遍历树中结点可以采用游标的方式。
A、在树中定义一个游标（GTreeNode<T>* node）
B、遍历开始前将游标指向根结点
C、获取游标指向的数据元素
D、通过结点中的 m_children 成员移动游标
![/assets/404.png](/assets/28995567.png)
将根结点压入队列中

```
 bool begin()
    {
      bool ret = (root() != NULL);
      if(ret)
      {
          //清空队列
          m_queue.clear();
          //根节点加入队列
          m_queue.add(root());
      }
      return ret;
    }
```

判断队列是否为空

```
 bool end()
    {
        return (m_queue.length() == 0);
    }
```

队头元素弹出，将队头元素的孩子压入队列中

```
 bool next()
    {
      bool ret = (m_queue.length() > 0);
      if(ret)
      {
          GTreeNode<T>* node = m_queue.front();
          m_queue.remove();//队头元素出队
          //将队头元素的子结点入队
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              m_queue.add(node->m_children.current());
          }
      }
      return ret;
    }
```

访问队头元素指向的数据元素

```
T current()
    {
      if(!end())
      {
          return m_queue.front()->value;
      }
      else
      {
          THROW_EXCEPTION(InvalidOperationException, "No value at current Node...");
        }
    }
```

## 四、通用树结构的实现

### 1、通用树节点的实现

```
 template <typename T>
  class GTreeNode:public TreeNode<T>
  {
  protected:
    bool m_flag;//堆空间标识
    //重载new操作符，声明为保护
    void* operator new(unsigned int size)throw()
    {
      return Object::operator new(size);
    }
    GTreeNode(const GTreeNode<T>& other);
    GTreeNode<T>& operator = (const GTreeNode<T>& other);

  public:
    LinkedList<GTreeNode<T>*> m_children;
    GTreeNode()
    {
      //栈上分配的空间标识为false
      m_flag = false;
    }
    //工厂方法，创建堆空间的结点
    static GTreeNode<T>* NewNode()
    {
      GTreeNode<T>* ret = new GTreeNode<T>();
      if(ret != NULL)
      {
          //堆空间的结点标识为true
          ret->m_flag = true;
      }
      return ret;
    }
    //堆空间结点标识访问函数
    bool flag()const
    {
      return m_flag;
    }
  };
```

### 2、通用树的实现

```
 template <typename T>
  class GTree:public Tree<T>
  {
  protected:
    LinkedQueue<GTreeNode<T>*> m_queue;
    GTree(const GTree<T>& other);
    GTree<T>& operator=(const GTree<T>& other);
    GTreeNode<T>* find(GTreeNode<T>* node, const T& value)const
    {
      GTreeNode<T>* ret = NULL;
      if(node != NULL)
      {
          //如果根结点的就是目标结点
          if(node->value == value)
          {
              ret =  node;
          }
          else
          {
              //遍历根节点的子结点
              for(node->m_children.move(0); !node->m_children.end() && (ret == NULL); node->m_children.next())
              {
                  //对每个子子结点进行查找
                  ret = find(node->m_children.current(), value);
              }
          }
      }
      return ret;
    }
    GTreeNode<T>* find(GTreeNode<T>* node, GTreeNode<T>* obj)const
    {
      GTreeNode<T>* ret = NULL;
      //根结点为目标结点
      if(node == obj)
      {
          ret =  node;
      }
      else
      {
          if(node != NULL)
          {
              //遍历子结点
              for(node->m_children.move(0); !node->m_children.end() && (ret == NULL); node->m_children.next())
              {
                  ret = find(node->m_children.current(), obj);
              }
          }
      }
      return ret;
    }

    void free(GTreeNode<T>* node)
    {
      if(node != NULL)
      {
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              free(node->m_children.current());
          }
          //如果结点存储在堆空间
          if(node->flag())
             delete node;//释放
      }
    }

    void remove(GTreeNode<T>* node, GTree<T>*& ret)
    {
      ret = new GTree<T>();
      if(ret != NULL)
      {
          //如果删除的结点是根结点
          if(root() == node)
          {
              this->m_root = NULL;
          }
          else
          {
              //获取删除结点的父结点的子结点链表
              LinkedList<GTreeNode<T>*>& child = dynamic_cast<GTreeNode<T>*>(node->parent)->m_children;
              //从链表中删除结点
              child.remove(child.find(node));
              //结点的父结点置NULL
              node->parent = NULL;
          }
          //将删除结点赋值给创建的树ret的根结点
          ret->m_root = node;
      }
      else
      {
          THROW_EXCEPTION(NoEnoughMemoryException, "No enough memory...");
      }
    }

    int count(GTreeNode<T>* node) const
    {
      int ret = 0;
      if(node != NULL)
      {
          ret = 1;//根结点
          //遍历根节点的子结点
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              ret += count(node->m_children.current());
          }
      }
      return ret;
    }

    int height(GTreeNode<T>* node)const
    {
      int ret = 0;
      if(node != NULL)
      {
          //遍历子结点
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              //当前结点的高度
              int h = height(node->m_children.current());
              if(ret < h)
              {
                  ret = h;
              }
          }
          ret = ret + 1;
      }
      return ret;
    }

    int degree(GTreeNode<T>* node) const
    {
      int ret = 0;
      if(node != NULL)
      {
          //结点的子结点的数量
          ret = node->m_children.length();
          //遍历子结点
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              int d = degree(node->m_children.current());
              if(ret < d)
              {
                  ret = d;
              }
          }
      }
      return ret;
    }
  public:
    GTree()
    {

    }
    //插入结点
    bool insert(TreeNode<T>* node)
    {
      bool ret = true;
      if(node != NULL)
      {
          //树为空，插入结点为根结点
          if(this->m_root == NULL)
          {
              node->parent = NULL;
              this->m_root = node;
          }
          else
          {
              //找到插入结点的父结点
              GTreeNode<T>* np = find(node->parent);
              if(np != NULL)
              {
                  GTreeNode<T>* n = dynamic_cast<GTreeNode<T>*>(node);
                  //如果子结点中无该结点，插入结点
                  if(np->m_children.find(n) < 0)
                  {
                      ret = np->m_children.insert(n);
                  }
              }
              else
              {
                  THROW_EXCEPTION(InvalidOperationException, "Invalid node...");
              }
          }
      }
      else
      {
          THROW_EXCEPTION(InvalidParameterException, "Parameter is invalid...");
      }
      return ret;
    }
    bool insert(const T& value, TreeNode<T>* parent)
    {
      bool ret = true;
      GTreeNode<T>* node = GTreeNode<T>::NewNode();
      if(node != NULL)
      {
          node->value = value;
          node->parent = parent;
          insert(node);
      }
      else
      {
          THROW_EXCEPTION(NoEnoughMemoryException, "No enough memory...");
      }
      return ret;
    }
    //删除结点
    SharedPointer<Tree<T>> remove(const T& value)
    {
      GTree<T>* ret = NULL;
      //找到结点
      GTreeNode<T>* node = find(value);
      if(node != NULL)
      {
          remove(node, ret);
          m_queue.clear();
      }
      else
      {
          THROW_EXCEPTION(InvalidParameterException, "Parameter invalid...");
      }
      return ret;
    }

    SharedPointer<Tree<T>> remove(TreeNode<T>* node)
    {
      GTree<T>* ret = NULL;
      node = find(node);
      if(node != NULL)
      {
          remove(dynamic_cast<GTreeNode<T>*>(node), ret);
          m_queue.clear();
      }
      else
      {
          THROW_EXCEPTION(InvalidParameterException, "Parameter invalid...");
      }
      return ret;
    }
    //查找结点
    GTreeNode<T>* find(const T& value)const
    {
      return find(root(), value);
    }
    GTreeNode<T>* find(TreeNode<T>* node)const
    {
        return find(root(), dynamic_cast<GTreeNode<T>*>(node));
    }
    //根结点访问函数
    GTreeNode<T>* root()const
    {
        return dynamic_cast<GTreeNode<T>*>(this->m_root);
    }
    //树的度访问函数
    int degree()const
    {
        return degree(root());
    }
    //树的高度访问函数
    int height()const
    {
        height(root());
    }
    //树的结点数目访问函数
    int count()const
    {
        count(root());
    }
    //清空树
    void clear()
    {
        free(root());
        this->m_root = NULL;
    }

    bool begin()
    {
      bool ret = (root() != NULL);
      if(ret)
      {
          //清空队列
          m_queue.clear();
          //根节点加入队列
          m_queue.add(root());
      }
      return ret;
    }

    bool end()
    {
        return (m_queue.length() == 0);
    }

    bool next()
    {
      bool ret = (m_queue.length() > 0);
      if(ret)
      {
          GTreeNode<T>* node = m_queue.front();
          m_queue.remove();//队头元素出队
          //将队头元素的子结点入队
          for(node->m_children.move(0); !node->m_children.end(); node->m_children.next())
          {
              m_queue.add(node->m_children.current());
          }
      }
      return ret;
    }

    T current()
    {
      if(!end())
      {
          return m_queue.front()->value;
      }
      else
      {
          THROW_EXCEPTION(InvalidOperationException, "No value at current Node...");
      }
    }
    virtual ~GTree()
    {
      clear();
    }
  };
```