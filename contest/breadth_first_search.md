- [BFS算法模板与练习](#bfs算法模板与练习)
    - [模板](#模板)
  - [献给阿尔吉侬的花束](#献给阿尔吉侬的花束)
  - [交换瓶子](#交换瓶子)
    - [暴力思路](#暴力思路)
    - [BFS图论思路](#bfs图论思路)


# BFS算法模板与练习

首先，计算机中常用的数据结构是栈和队列。

+ 栈：先进后出，通常应用是递归，DFS。

+ 队列：先进先出，通常应用是 BFS 。

过程如下所示：

每次取出队头元素，并且把其拓展的元素放在队尾。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/20-14-22-36-1679293354.png)

上面过程可知，遍历的过程以及入队的过程都是按照BFS(1 2 3...10)的顺序进行的

> BFS宽搜：每次扩展最早的点。（因此可以找到一条最短的路径）
> 
> DFS深搜：每次扩展第一个点。

BFS中常见问题，迷宫问题。

### 模板

1.判重

入队时判重，保证每个边只会入队一次，从而保证时间复杂度是线性的。（因此有判重数组的存在，宽搜也可以搜索环），st[ ]。

2.队列

```cpp
queue <--- 初始状态 // 队列保存初始状态
while(queue 非空)
{
    t < --- 队头 // t保存队头
    for(拓展t)
    {
        ver < --- 新节点 // 拓展t得到的新节点
        if(!st[ver]) // 如果拓展的新节点没有被搜索过
        {
            ver ----> 队尾 // 保存至队尾
        }
    }
}
```

## 献给阿尔吉侬的花束

阿尔吉侬是一只聪明又慵懒的小白鼠，它最擅长的就是走各种各样的迷宫。

今天它要挑战一个非常大的迷宫，研究员们为了鼓励阿尔吉侬尽快到达终点，就在终点放了一块阿尔吉侬最喜欢的奶酪。

现在研究员们想知道，如果阿尔吉侬足够聪明，它最少需要多少时间就能吃到奶酪。

迷宫用一个 R×C 的字符矩阵来表示。

字符 S 表示阿尔吉侬所在的位置，字符 E 表示奶酪所在的位置，字符 # 表示墙壁，字符 . 表示可以通行。

阿尔吉侬在 1 个单位时间内可以从当前的位置走到它上下左右四个方向上的任意一个位置，但不能走出地图边界。

**输入格式**

第一行是一个正整数 T，表示一共有 T 组数据。

每一组数据的第一行包含了两个用空格分开的正整数 R 和 C，表示地图是一个 R×C 的矩阵。

接下来的 R 行描述了地图的具体内容，每一行包含了 C 个字符。字符含义如题目描述中所述。保证有且仅有一个 S 和 E。

**输出格式**

对于每一组数据，输出阿尔吉侬吃到奶酪的最少单位时间。

若阿尔吉侬无法吃到奶酪，则输出“oop!”（只输出引号里面的内容，不输出引号）。

每组数据的输出结果占一行。

**数据范围**

$1<T≤10$,  
$2≤R,C≤200$

**输入样例：**

```
3
3 4
.S..
###.
..E.
3 4
.S..
.E..
....
3 4
.S..
####
..E.
```

**输出样例：**

```
5
1
oop!
```

code

```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

#define x first
#define y second

using namespace std;
// pair 有两个属性，first 和 second ，建议宏定义为x和y，方便理解
typedef pair<int, int> PII;

const int N = 210;

int n, m;
// 存储地图
char g[N][N];
// 存储坐标
int dist[N][N];

int bfs(PII start, PII end)
{
    queue<PII> q;
    memset(dist, -1, sizeof dist);

    dist[start.x][start.y] = 0;
    q.push(start);

    // 存储坐标位移
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    while (q.size())
    {
        auto t = q.front();
        q.pop();
        // 遍历坐标
        for (int i = 0; i < 4; i ++ )
        {
            int x = t.x + dx[i], y = t.y + dy[i];
            if (x < 0 || x >= n || y < 0 || y >= m) continue;  // 出界
            if (g[x][y] == '#') continue;  // 障碍物
            if (dist[x][y] != -1) continue;  // 之前已经遍历过

            dist[x][y] = dist[t.x][t.y] + 1;

            if (end == make_pair(x, y)) return dist[x][y];

            q.push({x, y});
        }
    }
    return -1;
}

int main()
{
    int T;
    scanf("%d", &T);
    while (T -- )
    {
        scanf("%d%d", &n, &m);
        // 一共读取n行，每行是一个一维字符串
        for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);
        // 不用定义成全局变量，如果定义全局变量会造成关键字冲突。
        // 如果想定义为全局变量，可以考虑换个名称例如 end1 等。
        PII start, end;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == 'S') start = {i, j};
                else if (g[i][j] == 'E') end = {i, j};

        int distance = bfs(start, end);
        if (distance == -1) puts("oop!");
        else printf("%d\n", distance);
    }

    return 0;
}
```

## 交换瓶子

有 N 个瓶子，编号 1∼N，放在架子上。

比如有 5 个瓶子：

```
2 1 3 5 4
```

要求每次拿起 2 个瓶子，交换它们的位置。

经过若干次后，使得瓶子的序号为：

```
1 2 3 4 5
```

对于这么简单的情况，显然，至少需要交换 2 次就可以复位。

如果瓶子更多呢？你可以通过编程来解决。

**输入格式**

第一行包含一个整数 N，表示瓶子数量。

第二行包含 N 个整数，表示瓶子目前的排列状况。

**输出格式**

输出一个正整数，表示至少交换多少次，才能完成排序。

**数据范围**

$1≤N≤10000,$

**输入样例1：**

```
5
3 1 2 5 4
```

**输出样例1：**

```
3
```

**输入样例2：**

```
5
5 4 3 2 1
```

**输出样例2：**

```
2
```

### 暴力思路

这道题可以采用暴力思路，通过观察可以发现，我们每一个数都必须回到它自己的位置上，比如 1 必须在第一位，2 必须在第二位上。由于每个数必须回到自己的位置，直接从 1 枚举到 n，如果当前位置的数不等于它的下标，那么我们就必须要把它给替换掉。设当前位置为 i
的话，那么我们就从 i+1开始往后枚举，直到找到对应的 a[j] 和我们的 i 相等，那么我们就把上个数交换，把交换次数++。由于每个瓶子都要归位，因此不会出现多余的步骤，可知是最少的次数。

code

```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int N = 10010;
int a[N], sum;

int main()
{
    int n;
    cin >> n;
    
    for(int i = 1; i <= n; i ++ ) scanf("%d", &a[i]);
    
    for(int i = 1; i <= n; i ++ )
        if(a[i] != i)
            for(int j = i + 1; j <= n; j ++ )
                if(a[j] == i) 
                {
                    swap(a[j], a[i]);
                    sum ++ ;
                    break;
                }
    cout << sum << endl;
    return 0;
}
```



### BFS图论思路

初始状态如下所示：

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/20-15-25-50-1679297142.png)

其中，每个位置向所在的瓶子连一条有向线。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/20-15-26-46-1679297205.png)

出度是1，入度是1，这样的一个环称为置换。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/20-15-27-14-1679297232.png)

最终希望的状态是变成五个自环。

**交换两个瓶子对于环产生的影响**

1. 交换同一个环内的点：裂成两个环。
   
   ![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/20-15-33-26-1679297605.png)

2. 交换不同环内的点：合并两个环。
   
   ![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/20-15-33-06-1679297584.png)

可见，交换瓶子实际上改变了位置连向瓶子的出边，也就是瓶子的入边。

分析题意，初始的时候有k个环，要将其变为n个环，每次操作最多增加一个环，因此最少需要 n - k 次操作才能完成。

算法复杂度为 $O(n)$ ，因为每个点被遍历常数次。

```cpp
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010;

int n;
// 瓶子的数量
int b[N];
// 判重数组帮助找环
bool st[N];

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &b[i]);

    int cnt = 0;
    for (int i = 1; i <= n; i ++ )
        if (!st[i])
        // 当前环没有被找过，说明在一个新环
        {
            cnt ++ ;
            // 把这个点能到达的点都标记一下
            for (int j = i; !st[j]; j = b[j])
                st[j] = true;
        }

    printf("%d\n", n - cnt);

    return 0;
}
```
