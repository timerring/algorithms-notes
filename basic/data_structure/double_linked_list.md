- [双链表图解及模板总结](#双链表图解及模板总结)
    - [双链表的参数](#双链表的参数)
    - [双链表的初始化](#双链表的初始化)
    - [节点k的右边插入一个数x](#节点k的右边插入一个数x)
      - [在k的左边插入一个数](#在k的左边插入一个数)
    - [删除节点k](#删除节点k)
    - [例题：双链表](#例题双链表)
      - [code](#code)
  - [模板总结](#模板总结)


# 双链表图解及模板总结

双向链表也叫双链表，是链表的一种，它的每个数据结点中都有两个指针，分别指向直接后继和直接前驱。所以，从双向链表中的任意一个结点开始，都可以很方便地访问它的前驱结点和后继结点。一般我们都构造双向循环链表。

用数组模拟双链表与单链表类似，都需要设置该点的值e[N]，以及一个指向左节点的指针l[N]，以及指向右节点的指针r[N]。

### 双链表的参数

```cpp
// e[]表示节点的值，l[]表示节点的左指针，r[]表示节点的右指针，idx表示当前用到了哪个节点
int e[N], l[N], r[N], idx;
```

### 双链表的初始化

双链表初始时head节点r[N]指向tail节点，tail节点l[N]指向head节点。示意图如下：

![image-20221018154548357](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221018154548357.png)

```cpp
// 初始化
void init()
{
    // 0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    idx = 2;
}
```

### 节点k的右边插入一个数x

先把新节点的左右两个指针建好（顺序不影响）。

![image-20221018155251390](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221018155251390.png)

再调整原来的指针，注意，应该**先改变下一个节点的左指针**，指向新的节点，然后改变k的右指针指向新节点。

![image-20221018155718542](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221018155718542.png)

![image-20221018155801226](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221018155801226.png)

```cpp
// 在节点k的右边插入一个数x
void insert(int k, int x)
{
    e[idx] = x;
    l[idx] = k, r[idx] = r[k];
    l[r[k]] = idx, r[k] = idx ++;
}
```

从代码可见，如果先改变了k的r[k]指针，则k下一个节点的左节点就无法表示。因为以及从k断开了，找不到原来k的下一个节点了。

#### 在k的左边插入一个数

基本思想同上，只不过可以调用函数时改为 `insert(l[k], x)` 。

### 删除节点k

删除节点较为容易，直接将左右指针跳过该点即可。

+ k的下一个节点的左指针指向k的左指针。
+ k的上一个节点的右指针指向k的右指针。

![image-20221018161044981](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221018161044981.png)

```cpp
// 删除节点k
void remove(int k)
{
    l[r[k]] = l[k];
    r[l[k]] = r[k];
}
```

> 同样的方式，如果用结构体来写的话就会非常难懂，不推荐。
> 
> 例如：
> 
> ```cpp
> struct Node
> {
>     int e,l,r;
> }node[N];
> // 非常复杂
> nodes[nodes[k].l].r = node[k].r
> ```

### 例题：双链表

实现一个双链表，双链表初始为空，支持 5 种操作：

1. 在最左侧插入一个数；
2. 在最右侧插入一个数；
3. 将第 k 个插入的数删除；
4. 在第 k 个插入的数左侧插入一个数；
5. 在第 k 个插入的数右侧插入一个数

现在要对该链表进行 M 次操作，进行完所有操作后，从左到右输出整个链表。

**注意**:题目中第 k 个插入的数并不是指当前链表的第 k 个数。例如操作过程中一共插入了 n 个数，则按照插入的时间顺序，这 n 个数依次为：第 1 个插入的数，第 2 个插入的数，…第 n 个插入的数。

**输入格式**

第一行包含整数 M，表示操作次数。

接下来 M 行，每行包含一个操作命令，操作命令可能为以下几种：

1. `L x`，表示在链表的最左端插入数 x。
2. `R x`，表示在链表的最右端插入数 x。
3. `D k`，表示将第 k 个插入的数删除。
4. `IL k x`，表示在第 k 个插入的数左侧插入一个数。
5. `IR k x`，表示在第 k 个插入的数右侧插入一个数。

**输出格式**

共一行，将整个链表从左到右输出。

**数据范围**

1≤M≤100000
所有操作保证合法。

**输入样例**：

```
10
R 7
D 1
L 3
IL 2 10
D 3
IL 2 7
L 8
R 9
IL 4 7
IR 2 2
```

**输出样例**：

```
8 7 7 3 2 9
```

#### code

```cpp
#include <iostream>

using namespace std;
const int N  = 100010;
int e[N], l[N], r[N], idx;

void init()
{
    l[1] = 0, r[0] = 1;
    idx = 2; // 有节点 0 和 1 ，因此从 2 开始
}

// 在节点k的右边插入一个数x
void insert(int k, int x)
{
    e[idx] = x;
    l[idx] = k, r[idx] = r[k];
    l[r[k]] = idx, r[k] = idx ++;
}

// 删除节点k
void remove(int k)
{
    l[r[k]] = l[k];
    r[l[k]] = r[k];

}

int main()
{
    int x, k, m;
    string op;
    init();
    cin >> m;
    while(m --)
    {
        cin >> op;
        if(op == "L"){
            cin >> x;
            insert(0, x);
        }
        if(op == "R"){
            cin >> x;
            insert(l[1], x);
        }
        if(op == "D"){
            cin >> k;
            remove(k + 1);
        }
        if(op == "IL"){
            cin >> k >> x;
            insert(l[k + 1], x);
        }
        if(op == "IR"){
            cin >> k >> x;
            insert(k + 1, x);
        }
    }

    for (int i = r[0]; i != 1; i = r[i]) cout << e[i] << ' ';

    return 0;
}
```

1. 之所以在 “D”, “IL”, “IR” 要用 k+1 的原因是 双链表的起始点是2。所以，每个插入位置k的真实位置应该为 k-1+2 = k+1 (在单链表中为 k-1，这里k-1指的是下标，因为单链表中下标是从0开始的，第k个数的下表就是k-1)，同样，由于在双链表中下标是从2开始的，因此实际的下标就是k+1。
2. 0, 1 节点的作用是边界。0为左边界，1为右边界。他俩在这里有点类似保留字的作用。正因如此，我们的idx也是从2开始
3. 最后遍历输出结果的 for (int i = rn[0]; i != 1; i = rn[i])。从 rn[0] 开始是因为 0 为左边界，而终止条件 i==1是因为1为右边界（如果碰到，说明已经遍历完毕） 

## 模板总结

```cpp
// e[]表示节点的值，l[]表示节点的左指针，r[]表示节点的右指针，idx表示当前用到了哪个节点
int e[N], l[N], r[N], idx;

// 初始化
void init()
{
    //0是左端点，1是右端点
    r[0] = 1, l[1] = 0;
    idx = 2;
}

// 在节点a的右边插入一个数x
void insert(int a, int x)
{
    e[idx] = x;
    l[idx] = a, r[idx] = r[a];
    l[r[a]] = idx, r[a] = idx ++ ;
}

// 删除节点a
void remove(int a)
{
    l[r[a]] = l[a];
    r[l[a]] = r[a];
}
```
