- [双指针算法模板及练习](#双指针算法模板及练习)
  - [日志统计](#日志统计)
    - [一般做法](#一般做法)
    - [用双指针算法改进](#用双指针算法改进)


# 双指针算法模板及练习

## 日志统计

小明维护着一个程序员论坛。现在他收集了一份”点赞”日志，日志共有 N 行。

其中每一行的格式是：

```
ts id
```

表示在 ts 时刻编号 id 的帖子收到一个”赞”。

现在小明想统计有哪些帖子曾经是”热帖”。

如果一个帖子曾在任意一个长度为 D 的时间段内收到不少于 K 个赞，小明就认为这个帖子曾是”热帖”。

具体来说，如果存在某个时刻 T 满足该帖在 [T,T+D) 这段时间内(注意是左闭右开区间)收到不少于 K 个赞，该帖就曾是”热帖”。

给定日志，请你帮助小明统计出所有曾是”热帖”的帖子编号。

**输入格式**

第一行包含三个整数 N,D,K。

以下 N 行每行一条日志，包含两个整数 ts 和 id。

**输出格式**

按从小到大的顺序输出热帖 id。

每个 id 占一行。

**数据范围**

$1≤K≤N≤10^5$,  
$0≤ts,id≤10^5,$  
$1≤D≤10000$

**输入样例：**

```
7 10 2
0 1
0 10
10 10
10 1
9 1
100 3
100 3
```

**输出样例：**

```
1
3
```

### 一般做法

```cpp
for(time) // 遍历所有的时间段
{
    memset(cnt, 0, sizeof cnt);
    for (id) // 遍历所有的帖子
    {
        cnt[id] ++ ; // 统计次数
        if(cnt[id] >= k) st[id] = true;
    }
}

for( int i = 1; i <= 100000; i ++ )
{
    if(st[i]) cout << i <<endl;
}
```

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/20-14-05-34-1679292328.png)

### 用双指针算法改进

每次只需要变动 i（首）和 j （尾）即可，中间的部分没有变化。

```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

// 此题需要对二元组进行排序，需要针对时间和id进行排序。
// 可以用pair来sort排序，默认是以first为第一关键字，second为第二关键字排序
typedef pair<int, int> PII;

const int N = 100010;

int n, d, k;
PII logs[N];
int cnt[N];  // 用来记录某id号获得的赞数，表示形式为cnt[id]++;
bool st[N];  // 记录每个帖子是否是热帖, 因为id <= 1e5，所以可以利用遍历来输出。

int main()
{
    scanf("%d%d%d", &n, &d, &k);
    for (int i = 0; i < n; i ++ ) scanf("%d%d", &logs[i].x, &logs[i].y);

    // 对pair进行排序, 排序时默认以first为准排序
    sort(logs, logs + n);
    // 双指针算法， i在前，j在后
    for (int i = 0, j = 0; i < n; i ++ )
    {
        int id = logs[i].y; // 把每个获赞的帖子id存入id
        cnt[id] ++ ; // 获得一个赞，所以此刻 ++；

        while (logs[i].x - logs[j].x >= d) // 如果俩个帖子时间相差超过d，说明该赞无效
        {
            cnt[logs[j].y] -- ; // 所以此刻--；
            j ++ ; // 要把指针j往后，否则死循环
        }

        if (cnt[id] >= k) st[id] = true;  // 如果该id贴赞超过k，说明是热帖
    }

    for (int i = 0; i <= 100000; i ++ ) // 循环输出热帖
        if (st[i])
            printf("%d\n", i);

    return 0;
}
```
