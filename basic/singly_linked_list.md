# 单链表图解及模板总结

## 静态链表

如果说用结构体+指针的方式实现链表和栈的话，每次需要new一个新节点，非常慢。笔试题链表大小在10万级别，因此笔试题一般不会采用这种动态链表的方式。通常采用数组模拟链表的方式，这种方式更快。

```cpp
struct Node
{
    int val;
    Node *next;
};
new Node();
```

## 链表与邻接表

## 邻接表

邻接表的 `head[i]` 存储的是节点 `i` 所有的临边。实际上就是一个单链表。

### 用数组模拟单链表

单链表里常用的是邻接表。邻接表主要用来存储图和数。

双链表常用来优化某些问题。

链表的结构通常如下所示：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221017182739361.png)

每一个节点中通常存储两个数，分别是节点值（val）和指向下一个节点的指针（next）。通常如下：

+ val ：e[N]
+ next ：ne[N]

两者通过索引下标相联系。

<!-- more -->

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221017183223725.png)

```cpp
#include<iostream>
using namespace std;

const int N = 100010;

// head 表示头节点的下标
// e[i] 表示节点i的值
// ne[i] 表示节点i的next指针是多少
// idx存储当前用到了哪个点
int head, e[N], ne[N], idx;
```

#### 初始化操作

```cpp
//初始化
void init()
{
    head = -1;idx = 0;
}
```

#### 插入头节点

基本思路如下：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221017211027847.png)

```cpp
//将x插到头结点
void add_to_head(int x)
{
    // 把插入的值x存在idx中
    e[idx] = x;
    // 让当前的idx的next指针与头节点指向一致
    ne[idx] = head;
    // head指向新节点idx
    head = idx;
    idx ++;
}
```

这里也可以将代码压缩一下，更加简洁

```cpp
//将x插到头结点
void add_to_head(int x)
{
    e[idx] = x, ne[idx] = head, head = idx, idx ++;
}
```

#### 插入下标为k的后面

```cpp
//将x插入下标为k的后面
void add(int k, int x)
{
    e[idx] = x, ne[idx] = ne[k], ne[k] = idx, idx ++;
}
```

当然也可以进一步化简，将`ne[k] = idx, idx ++;`化简为`ne[k] = idx++`。

#### 删除操作

将下标为k的点后面的点删除：只需要指针跳过该点即可。

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221017211610701.png)

```cpp
void remove(int k)
{
    // k的指向等于它下一个的节点的指向
    ne[k] = ne[ne[k]];
}
```

#### 例题：单链表

实现一个单链表，链表初始为空，支持三种操作：

1. 向链表头插入一个数；
2. 删除第 k 个插入的数后面的数；
3. 在第 k 个插入的数后插入一个数。

现在要对该链表进行 M 次操作，进行完所有操作后，从头到尾输出整个链表。

**注意**:题目中第 k 个插入的数并不是指当前链表的第 k 个数。例如操作过程中一共插入了 n 个数，则按照插入的时间顺序，这 n 个数依次为：第 1 个插入的数，第 2 个插入的数，…第 n 个插入的数。

**输入格式**

第一行包含整数 M，表示操作次数。

接下来 M 行，每行包含一个操作命令，操作命令可能为以下几种：

1. `H x`，表示向链表头插入一个数 x。
2. `D k`，表示删除第 k 个插入的数后面的数（当 k 为 0 时，表示删除头结点）。
3. `I k x`，表示在第 k 个插入的数后面插入一个数 x（此操作中 k 均大于 0）。

**输出格式**

共一行，将整个链表从头到尾输出。

**数据范围**

$1≤M≤100000$
所有操作保证合法。

**输入样例**：

```
10
H 9
I 1 1
D 1
D 0
H 6
I 3 6
I 4 5
I 4 5
I 3 4
D 6
```

**输出样例**：

```
6 4 6 5
```

##### code

此题需要注意的是，题目要求在第k个节点上进行操作，而第k个节点的下标是k-1。

```cpp
#include<iostream>
using namespace std;

const int M = 100010;

int head, idx, e[M], ne[M];

void init()
{
    head = -1;
    idx = 0;
}

void add_to_head(int x)
{
    e[idx] = x, ne[idx] = head, head = idx ++ ;
}

void add(int k, int x)
{
    e[idx] = x, ne[idx] = ne[k], ne[k] = idx ++ ;
}

void remove(int k)
{
    ne[k] = ne[ne[k]];
}

int main()
{
    int m;
    cin >> m;

    init();

    while(m --)
    {
        int k, x;
        char op;

        cin >> op;
        if(op == 'H')
        {
            cin >> x;
            add_to_head(x);
        }
        else if(op == 'D')
        {
            cin >> k;
            // 要特别注意删除节点头的状况
            if(!k)head = ne[head];
            remove(k - 1);
        }
        else
        {
            cin >> k >> x;
            add(k - 1, x);
        }
    }
    for(int i = head; i != -1; i = ne[i]) cout << e[i] << ' ';
    return 0;
}
```

## 单链表模板总结

```cpp
// head存储链表头，e[]存储节点的值，ne[]存储节点的next指针，idx表示当前用到了哪个节点
int head, e[N], ne[N], idx;

// 初始化
void init()
{
    head = -1;
    idx = 0;
}

// 在链表头插入一个数a
void insert(int a)
{
    e[idx] = a, ne[idx] = head, head = idx ++ ;
}

// 将头结点删除，需要保证头结点存在
void remove()
{
    head = ne[head];
}
```
