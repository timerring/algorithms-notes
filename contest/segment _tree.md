- [线段树模板及练习](#线段树模板及练习)
  - [思想](#思想)
  - [核心函数](#核心函数)
  - [存储方式](#存储方式)
    - [模板：动态求连续区间和](#模板动态求连续区间和)
    - [练习：数列区间最大值](#练习数列区间最大值)


# 线段树模板及练习

应用：求区间最大值，求染色面积，长度，最大连续和等等。

## 思想

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/19-09-59-41-1679191168.png)

操作一：单点修改 ($O(logn)$)

单点修改的基础思想就是仅修改信息需要变化的节点，类似一个递归 + 回溯的过程。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/19-10-05-06-1679191504.png)

操作二：区间查询 ($O(logn)$)

查询也是一个递归的过程，如果查询的区间已经把当前区间完全包含了，则可以返回该区间了。

> 此外，进行区间修改往往会使用懒标记。

## 核心函数

pushup：用子节点信息更新当前节点信息

build：在一段区间上初始化线段树

modify：修改

query：查询

（此外，懒标记会有pushdown）

## 存储方式

线段数组的存储方式与堆（heap）的存储方式类似，采用了一维数组：

对于节点x：

+ 父节点为 $\lfloor\frac{x}{2}\rfloor$，记为 `x >> 1`

+ 左儿子为2x，记为 `x << 1`

+ 右儿子为2x + 1，记为 `x << 1 | 1`

关于数组存储的空间问题：最理想情况是 $N+\frac{N}{2}+\frac{N}{4}+…=2N−1$，一般情况下最后一层节点不是满的，但最坏情况也不会超过前面整个二叉树的节点总数，也就是不会超过 2N−1，所以一共最多不会超过 4N。这里N 如果不是2的整次幂就是倒数第二层，否则就是最后一层。

### 模板：动态求连续区间和

给定 n 个数组成的一个数列，规定有两种操作，一是修改某个元素，二是求子数列 [a,b] 的连续和。

**输入格式**

第一行包含两个整数 n 和 m，分别表示数的个数和操作次数。

第二行包含 n 个整数，表示完整数列。

接下来 m 行，每行包含三个整数 k,a,b （k=0，表示求子数列[a,b]的和；k=1，表示第 a 个数加 b）。

数列从 1 开始计数。

**输出格式**

输出若干行数字，表示 k=0 时，对应的子数列 [a,b] 的连续和。

**数据范围**

$1≤n≤100000$,  
$1≤m≤100000$，  
$1≤a≤b≤n$,  
数据保证在任何时候，数列中所有元素之和均在 int 范围内。

**输入样例：**

```
10 5
1 2 3 4 5 6 7 8 9 10
1 1 5
0 1 3
0 4 8
1 7 5
0 4 8
```

**输出样例：**

```
11
30
35
```

code

```cpp
#include<iostream>
#include<cstring>
#include<cstdio>
#include<algorithm>

using namespace std;

const int N=100010;

int n,m;

int w[N];//记录一下权重

struct node{
    int l,r;//左右区间

    int sum;//总和
}tr[N*4];//记得开 4 倍空间

void push_up(int u)//利用它的两个子节点来算一下它的当前节点信息
{
    tr[u].sum=tr[u<<1].sum+tr[u<<1|1].sum;//左儿子 u<<1 ,右儿子 u<<1|1  
}

void build(int u,int l,int r)/*第一个参数，当前节点编号，第二个参数，左边界，第三个参数，右边界*/
{
    if(l==r)tr[u]={l,r,w[r]};//如果当前已经是叶节点了，那我们就直接赋值就可以了
    else//否则的话，说明当前区间长度至少是 2 对吧，那么我们需要把当前区间分为左右两个区间，那先要找边界点
    {
        tr[u]={l,r};//这里记得赋值一下左右边界的初值

        int mid=l+r>>1;//边界的话直接去计算一下 l + r 的下取整

        build(u<<1,l,mid);//先递归一下左儿子

        build(u<<1|1,mid+1,r);//然后递归一下右儿子

        push_up(u);//做完两个儿子之后的话呢 push_up 一遍u，更新一下当前节点信息
    }
}

int query(int u,int l,int r)//查询的过程是从根结点开始往下找对应的一个区间
{
    if(l<=tr[u].l && tr[u].r<=r)return tr[u].sum;//如果当前区间已经完全被包含了，那么我们直接返回它的值就可以了
    //否则的话我们需要去递归来算
    int mid=tr[u].l+tr[u].r>>1;//计算一下我们 当前 区间的中点是多少
    //先判断一下和左边有没有交集

    int sum=0;//用 sum 来表示一下我们的总和

    if(mid>=l)sum+=query(u<<1,l,r);//看一下我们当前区间的中点和左边有没有交集
    if(r>=mid+1)//看一下我们当前区间的中点和右边有没有交集
        sum+=query(u<<1|1,l,r);

    return sum;

}

void modify(int u,int x,int v)//第一个参数也就是当前节点的编号,第二个参数是要修改的位置，第三个参数是要修改的值
{
    if(tr[u].l==tr[u].r)tr[u].sum+=v; //如果当前已经是叶节点了，那我们就直接让他的总和加上 v 就可以了

    //否则
    else
    {

      int mid=tr[u].l+tr[u].r>>1;
      //看一下 x 是在左半边还是在右半边
      if(x<=mid)modify(u<<1,x,v);//如果是在左半边，那就找左儿子
      else modify(u<<1|1,x,v);//如果在右半边，那就找右儿子

      //更新完之后当前节点的信息就要发生变化对吧，那么我们就需要 pushup 一遍

      push_up(u);
    }
}

int main()
{
    scanf("%d%d",&n,&m);

    for(int i=1; i<=n; i++)scanf("%d",&w[i]);

    build(1, 1, n);/*第一个参数是根节点的下标，根节点是一号点，然后初始区间是 1 到 n */

    //后面的话就是一些修改操作了

    while(m--)
    {
        int k,a,b;

        scanf("%d%d%d",&k,&a,&b);

        if(!k)printf("%d\n",query(1,a,b));//求和的时候，也是传三个参数，第一个的话是根节点的编号 ，第二个的话是我们查询的区间 
        //第一个参数也就是当前节点的编号
        else
        modify(1,a,b);//第一个参数是根节点的下标,第二个参数是要修改的位置，第三个参数是要修改的值
    }
    return 0;
}
```

### 练习：数列区间最大值

输入一串数字，给你 M 个询问，每次询问就给你两个数字 X,Y，要求你说出 X 到 Y 这段区间内的最大数。

**输入格式**

第一行两个整数 N,M 表示数字的个数和要询问的次数；

接下来一行为 N 个数；

接下来 M 行，每行都有两个整数 X,Y。

**输出格式**

输出共 M 行，每行输出一个数。

**数据范围**

$1≤N≤10^5$,  
$1≤M≤10^6$,  
$1≤X≤Y≤N$,  
数列中的数字均不超过$2^{31}−1$

**输入样例：**

```
10 2
3 2 4 5 6 8 1 2 9 7
1 4
3 8
```

**输出样例：**

```
5
8
```

code

```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <climits>

using namespace std;

const int N = 100010;

int n, m;
int w[N];
struct Node
{
    int l, r;
    int maxv;
}tr[N * 4];

// u 表示根节点
void build(int u, int l, int r)
{
    if (l == r) tr[u] = {l, r, w[r]}; // 这里相等，w[r]和w[l]赋值哪个都行
    else
    {
        tr[u] = {l, r}; // 这样写默认在tr[]中，l,r分别进行了赋值，最后maxv没有赋值，默认为0
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        tr[u].maxv = max(tr[u << 1].maxv, tr[u << 1 | 1].maxv); // 回溯根节点的最大值，相当于把pushup写到build里了
    }
}

int query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].maxv;
    int mid = tr[u].l + tr[u].r >> 1; // 注意这里的中点一定是树的中点，而非查询的中点
    int maxv = INT_MIN;  // INT_MIN是limits.h里定义的的int类型的最小值
    if (l <= mid) maxv = query(u << 1, l, r); // 左边有交集，此时返回值一定大于等于INT_MIN
    if (r > mid) maxv = max(maxv, query(u << 1 | 1, l, r)); // 右边有交集，此时maxv可能已经改变，需要在当前最大值和查询结果中取最大值
    return maxv;
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);

    build(1, 1, n);

    int l, r;
    while (m -- )
    {
        scanf("%d%d", &l, &r);
        printf("%d\n", query(1, l, r));
    }

    return 0;
}
```
