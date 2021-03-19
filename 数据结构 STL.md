# vector
## 二维vector 两种初始化方式
```C++
vector<vector<char>>vec(row,vector<char>(col,'#'));
```
```C++
vector<vector<char>>vec1;
vec1.resize(row);
for(int i=0;i<vec1.size();i++)
    vec1[i].resize(col);
for(int i=0;i<row;i++)
    for(int j=0;j<col;j++)
        vec1[i][j]='#';
```
## 初始化
### 自定义
vector<int> a(10); //定义了10个整型元素的向量，但没有给出初值，其值是不确定的。
vector<int> a(10,1); //定义了10个整型元素的向量,且给出每个元素的初值为1
### 向量/数组copy
vector<int> a(b); //用b向量来创建a向量，整体复制性赋值
vector<int> a(b.begin(),b.begin+3); //定义了a值为b中第0个到第2个（共3个）元素
int b[7]={1,2,3,4,5,9,8}; vector<int> a(b,b+7); //从数组中获得初值
## 下标
a.front(); //返回a的第一个元素
a.back(); //返回a的最后一个元素
a[i]; //返回a的第i个元素，当且仅当a[i]存在
## 插入/删除
a.clear(); //清空a中的元素

a.push_back(5); //在a的最后一个向量后插入一个元素，其值为5
a.pop_back(); //删除a向量的最后一个元素

a.insert(a.begin()+1,5); //在a的第1个元素（从第0个算起）的位置插入数值5，如a为1,2,3,4，插入元素后为1,5,2,3,4
a.insert(a.begin()+1,3,5); //在a的第1个元素（从第0个算起）的位置插入3个数，其值都为5
a.insert(a.begin()+1,b+3,b+6); //b为数组，在a的第1个元素（从第0个算起）的位置插入b的第3个元素到第5个元素（不包括b+6），如b为1,2,3,**4,5,9**,8，a为1,2,3,4，插入元素后为1,**4,5,9**,2,3,4

vector<int>::iterator iter=a.begin();
a.erase(iter) //erase后该元素被擦除，剩余元素前进，iter指向的是被erase的下一个元素，实际上仍旧是当前位置
## 查找
vector本身是没有find这一方法，其find是依靠algorithm来实现的。
vector<int>::iterator iter=find(vec.begin(),vec.end(),7);//vec={8,7,6} 
int pos=iter-vec.begin();//pos=1 find返回类型为iterator，相减得到下标pos
int a=*iter;//a=7 *iter返回iter指向的值 此时iter指向7的位置，值为7

## 长度/容量
a.size(); //返回a中元素的个数；
a.resize(10); //将a的现有元素个数调至10个，多则删，少则补，值为类型默认初始值
a.resize(10,2); //将a的现有元素个数调至10个，多则删，少则补，其值为2

a.capacity(); //返回a在内存中总共可以容纳的元素个数
a.reserve(100); //将a的容量（capacity）扩充至100，这种操作只有在需要给a添加大量数据的时候才显得有意义，可以避免内存多次容量扩充操作（当a的容量不足时电脑会自动扩容，当然这必然降低性能） 
## 其他
a.empty(); //判断a是否为空，空则返回ture,不空则返回false
a.erase(a.begin()+1,a.begin()+3); //删除a中第1个（从第0个算起）到第2个元素，也就是说删除的元素从[a.begin()+1,a.begin()+3)
a.swap(b); //b为向量，将a中的元素和b中的元素进行整体性交换
a==b; //b为向量，向量的比较操作还有!=,>=,<=,>,<


# unordered_map
## 长度 myMap.size()
## 下标 myMap[i]="第i个value"
## 插入 myMap[i]="new value" or maMap.insert(pair(key,value))


# 迭代器 iterator
auto iter = myMap.begin();
iter=myMap.find(target-nums[i]);
if(iter!=myMap.end()){}


# queue
push
pop
size
empty
front
back

# deque
## 构造函数
**deque():创建一个空deque**
**deque(int nSize):创建一个deque,元素个数为nSize**
**deque(int nSize,const T& t):创建一个deque,元素个数为nSize,且值均为t**
**deque(const deque &):复制构造函数**

## 增加函数
**void push_front(const T& x):双端队列头部增加一个元素x**
**void push_back(const T& x):双端队列尾部增加一个元素x**

iterator insert(iterator it,const T& x):双端队列中某一元素前增加一个元素x
void insert(iterator it,int n,const T& x):双端队列中某一元素前增加n个相同的元素x
void insert(iterator it,const_iterator first,const_iteratorlast):双端队列中某一元素前插入另一个相同类型向量的[forst,last)间的数据

### 删除函数
Iterator erase(iterator it):删除双端队列中的某一个元素
Iterator erase(iterator first,iterator last):删除双端队列中[first,last）中的元素

**void pop_front():删除双端队列中最前一个元素**
**void pop_back():删除双端队列中最后一个元素**
**void clear():清空双端队列中所有元素**

### 遍历函数
**reference at(int pos):返回pos位置元素的引用**
**reference front():返回首元素的引用**
**reference back():返回尾元素的引用**

iterator begin():返回向量头指针，指向第一个元素
iterator end():返回指向向量中最后一个元素下一个元素的指针（不包含在向量中）
reverse_iterator rbegin():反向迭代器，指向最后一个元素
reverse_iterator rend():反向迭代器，指向第一个元素的前一个元素

### 判断函数
bool empty() const:向量是否为空，若true,则向量中无元素

### 大小函数
**Int size() const:返回向量中元素的个数**
int max_size() const:返回最大可允许的双端对了元素数量值

### 其他函数
void swap(deque&):交换两个同类型向量的数据
void assign(int n,const T& x):向量中第n个元素的值设置为x


# struct
## 初始化函数
```C++
typedef struct Test{
    int id;
    int name;
    // 用以不初始化就构造结构体
    Test(){} ;
    //只初始化id
    Test(int _name) {
        name = _name;
    }
    //同时初始化id,name
    Test(int _id,int _name): id(_id),name(_name){};
}Test;
```

## sizeof 对齐
结构体大小必须是所有成员大小的整数倍，也即所有成员大小的公倍数

Char      偏移量必须为sizeof(char)即1的倍数 
Short     偏移量必须为sizeof(short)即2的倍数 
int       偏移量必须为sizeof(int)即4的倍数 
float     偏移量必须为sizeof(float)即4的倍数 
double    偏移量必须为sizeof(double)即8的倍数 

```C++
struct s1
{
　char a;
　double b;
　int c;
　char d;
};

struct s2
{
　char a;
　char b;
　int c;
　double d;
};

cout<<sizeof(s1)<<endl; // 24
cout<<sizeof(s2)<<endl; // 16
```

# 树
#### 满二叉树
k层有2^k-1个节点 每一层都装满
#### 完全二叉树
k层，除了最后一层每一层都装满，最后一层节点从左往右排布
#### 二叉查找树 Binary Search Tree(BST)
左边节点小于根节点，右边节点大于等于根节点，中序遍历是递增的
#### 平衡查找树 Balanced Search Tree
平衡因子（Balance Factor, BF）：左子树高度与右子树高度之差
使用二叉搜索树对某个元素进行查找，虽然平均情况下的时间复杂度是 O(log n)，但是最坏情况下（当所有元素都在树的一侧时）的时间复杂度是 O(n)。因此有了平衡查找树，平均和最坏情况下的时间复杂度都是 O(log n)
平衡查找树有很多不同的实现方式：AVL 树、2-3查找树、伸展树、红黑树、B树、B+树

##### AVL 树/平衡二叉树（Balanced Binary Tree）
对于所有结点，BF 的绝对值小于等于1，即左、右子树的高度之差的绝对值小于等于1
可支持 O(log n) 的查找、插入、删除，它比红黑树严格意义上**更为平衡**，从而导致**插入和删除更慢**，但**遍历却更快**。
适合用于**只需要构建一次**，就可以在不重新构造的情况下读取的情况。
##### 2-3查找树
##### 伸展树 ？？
##### 红黑树 ？？
##### B树
##### B+树

# 并查集
https://blog.csdn.net/dm_vincent/article/details/7655764