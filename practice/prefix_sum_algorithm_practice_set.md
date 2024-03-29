- [前缀和算法练习集](#前缀和算法练习集)
  - [截断数组](#截断数组)
  - [K倍区间](#k倍区间)
  - [前缀和](#前缀和)
  - [子矩阵的和](#子矩阵的和)
  - [激光炸弹](#激光炸弹)

# 前缀和算法练习集

## 截断数组

给定一个长度为 n 的数组 a1,a2,…,an。

现在，要将该数组从中间截断，得到三个**非空**子数组。

要求，三个子数组内各元素之和都相等。

请问，共有多少种不同的截断方法？

**输入格式**

第一行包含整数 n。

第二行包含 n 个整数 a1,a2,…,an。

**输出格式**

输出一个整数，表示截断方法数量。

**数据范围**

前六个测试点满足 $1≤n≤10$。  
所有测试点满足 $1≤n≤10^5$，$−10000≤a_i≤10000$。

**输入样例1：**

```
4
1 2 3 3
```

**输出样例1：**

```
1
```

**输入样例2：**

```
5
1 2 3 4 5
```

**输出样例2：**

```
0
```

**输入样例3：**

```
2
0 0
```

**输出样例3：**

```
0
```

**code**

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;
typedef long long LL;

int n;
int s[N];

int main()
{

    cin >> n;
    for(int i = 1; i <= n; i ++ )
    {
        int x;
        cin >> x;
        s[i] = s[i - 1] + x;
    }
    // 每个数组的元素和为原数组的1/3
    if(s[n] % 3) puts("0");
    else
    {
        LL res = 0;
        // 寻找两刀的位置
        for(int j = 3, cnt = 0; j <= n; j ++ )
        {
            // 第一刀满足三分之一，则记录该刀, 第一刀范围[1, n - 2] (cnt的意思就是 j - 2 位置前面有多少可以切第一刀的地方)
            if(s[j - 2] == s[n] / 3) cnt ++;
            // 第二刀满足后半部分三分之一，如果满足，则可以组成答案(第二刀跟前面的cnt个第一刀组合，所以总个数增加cnt个)
            if(s[n] - s[j - 1] == s[n] / 3) res += cnt;
        }
    printf("%lld\n", res);
    }

    return 0;
}
```

## K倍区间

给定一个长度为 N 的数列，$A_1,A_2,…A_N$，如果其中一段连续的子序列 $A_i,A_{i+1},…A_j$ 之和是 K 的倍数，我们就称这个区间 [i,j] 是 K 倍区间。

你能求出数列中总共有多少个 K 倍区间吗？

输入格式

第一行包含两个整数 N 和 K。

以下 N 行每行包含一个整数 $A_i$。

输出格式

输出一个整数，代表 K 倍区间的数目。

数据范围

$1≤N,K≤100000$,  
$1≤A_i≤100000$

输入样例：

```
5 2
1
2
3
4
5
```

输出样例：

```
6
```

当`R`固定时，在 `0 ~ R - 1` 之间找到有多少个`L`满足`(s[R] - s[L]) % k == 0`，可转换为：`s[R] % k == s[L] % k`

即有多少个 `S[L]` 与 `S[R]` 的余数相同

我们可以开一个新的数组，`cnt[i]`，表示余数是`i`的数有多少个。

`cnt[0]`为什么要赋值为1？

因为我们的思路是找到两个序列和`s[R] % k`和`s[L] % k`的余数相同的个数，而我们的前缀和一般式不包含 `s[0]` 这个东西的，因为它的值是0，在前缀和中没有意义，但是这道题有意义，样例里面前缀和序列 %k 之后是1 1 0 0 1，两两比较，我们只能找到四个。

为什么少了两个？因为我们不一定需要两个序列，单个序列取余==0也构成k倍区间，此时我们就要假设`s[0] = 0`是有意义的；

我们`cnt[0]`中存的是`s[]`中等于0的数的个数，由于`s[0] = 0`，所以最初等于0的有1个数，所以`cnt[0] = 1` 。

这里我进行了验算，发现这种方式确实符合题意。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/25-20-07-08-1679746023.png)

cnt的意义：存储模k的值，将其作为左端点（模k左端点）的数量
for的意义：遍历每个端点，先将其作为模k右端点，根据其模k的值查看它有多少个模k左端点，即能形成多少个模k区间，然后将其作为模k左端点，数量加1
for 前 cnt[0] = 1的意义：当遍历出首个为k倍的前缀和时，它不需要模k左端点即可形成模k区间，为满足通解将0作为其模k左端点，故 cnt[0] ++。

**code：**

```cpp
#include<iostream>
using namespace std;
const int N=1000009;
typedef long long LL;
LL sum[N];
LL cnt[N];
int n,k;

int main()
{
    cin>> n >> k;
    for(int i=1;i<=n;i++)
    {
        scanf("%lld",&sum[i]);
        sum[i]=sum[i]+sum[i-1];
    }


    /*调试用，打印各求和区间的范围
    for(int i=1;i<=n;i++)
    {
        for(int j=0;j<=i-1;j++)
            printf("[%d,%d] ",j+1,i);
        cout<<endl;
    }
    cout<<endl;
    */
    LL res=0;
    // cnt[0]=1;
    /*
    当R固定时，在L在[0,R-1]之间，所有sum[R]与sum[L]余数相等的数的个
    数
    先统计，再累加，然后代码是从i=1开始循环,所以实际上res这一行执行了n次，
    而cnt执行了虽然也是n次，但是第n次时，统计的数已经没有意义了，所以只执行了
    [1,n-1]的有效范围，0被忽视掉了！
    所以必须加上cnt[0]=1这一句才满足“在L在[0,R-1]之间”（R指右边界，姑且认为是n）

    如果觉得太麻烦，可以试试先累加，再统计，便于理解，俩行已经在下面注释掉
    */
    for(int i=1;i<=n;i++)
    {
        // res+=cnt[sum[i]%k];//先统计 出现与当前值求余的结果相同的元素个数
        // cnt[sum[i]%k]++;//再累加当前的值求余的结果,并储存进cnt中

        //下面解法是先累加在统计，不用考虑cnt[]的影响
        //当R固定时，在L在[1,R]之间，所有sum[R]与sum[L-1]余数相等的数的个数

        cnt[sum[i-1]%k]++;//先累加，sum[i-1]代表sum[L-1]的余数，cnt[sum[i-1]%k]代表其余数的个数
        res+=cnt[sum[i]%k];//再统计,直接统计sum[i]就行了
                                //即判断所有sum[R]与sum[L]余数相等的数的个数

        /*调试用：打印各个步骤的值
        for(int i=0;i<k;i++)
            printf("%d ",cnt[i]);
        printf("res=%d\n",res);
        */
    }
    printf("%lld",res);
}
```

## 前缀和

输入一个长度为 n 的整数序列。

接下来再输入 m 个询问，每个询问输入一对 l,r。

对于每个询问，输出原序列中从第 l 个数到第 r 个数的和。

输入格式

第一行包含两个整数 n 和 m。

第二行包含 n 个整数，表示整数数列。

接下来 m 行，每行包含两个整数 l 和 r，表示一个询问的区间范围。

输出格式

共 m 行，每行输出一个询问的结果。

数据范围

$1≤l≤r≤n$,  
$1≤n,m≤100000$,  
$−1000≤数列中元素的值≤1000$

输入样例：

```
5 3
2 1 3 6 4
1 2
1 3
2 4
```

输出样例：

```
3
6
10
```

code

```cpp
#include <iostream>

using namespace std;

const int N = 100010;
int s[N];

int query(int s[], int l, int r)
{
    cout << s[r] - s[l - 1] << endl;
}

int main()
{
    int m, n;
    cin >> n >> m;

    for ( int i = 1; i <= n ; i ++ )
    {
        int x;
        cin >> x;
        s[i] = s[i - 1] + x;
    }

    while(m --)
    {
        int l, r;
        cin >> l >> r;
        query(s, l, r);
    }
    return 0;
}
```

## 子矩阵的和

输入一个 n 行 m 列的整数矩阵，再输入 q 个询问，每个询问包含四个整数 x1,y1,x2,y2，表示一个子矩阵的左上角坐标和右下角坐标。

对于每个询问输出子矩阵中所有数的和。

输入格式

第一行包含三个整数 n，m，q。

接下来 n 行，每行包含 m 个整数，表示整数矩阵。

接下来 q 行，每行包含四个整数 x1,y1,x2,y2，表示一组询问。

输出格式

共 q 行，每行输出一个询问的结果。

数据范围

1≤n,m≤1000,  
1≤q≤200000,  
1≤x1≤x2≤n,  
1≤y1≤y2≤m,  
−1000≤矩阵内元素的值≤1000

输入样例：

```
3 4 3
1 7 2 4
3 6 2 8
2 1 2 3
1 1 2 2
2 1 3 4
1 3 3 4
```

输出样例：

```
17
27
21
```

code

```cpp
#include <iostream>

using namespace std;

const int N = 1010;
int s[N][N];

int main()
{
    int n, m, q;
    cin >> n >> m >> q;

    for(int i = 1; i <= n; i ++)
        for(int j = 1; j <= m; j ++)
            cin >> s[i][j];

    for(int i = 1; i <= n; i ++)
        for(int j = 1; j <= m; j ++)
            s[i][j] += s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];

    while( q -- )
    {
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        cout << s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1] << endl;
    }
    return 0;
}
```

## 激光炸弹

地图上有 N 个目标，用整数 $X_i$,$Y_i$ 表示目标在地图上的位置，每个目标都有一个价值 $W_i$。

**注意**：不同目标可能在同一位置。

现在有一种新型的激光炸弹，可以摧毁一个包含 R×R 个位置的正方形内的所有目标。

激光炸弹的投放是通过卫星定位的，但其有一个缺点，就是其爆炸范围，即那个正方形的边必须和 x，y 轴平行。

求一颗炸弹最多能炸掉地图上总价值为多少的目标。

输入格式

第一行输入正整数 N 和 R，分别代表地图上的目标数目和正方形包含的横纵位置数量，数据用空格隔开。

接下来 N 行，每行输入一组数据，每组数据包括三个整数 Xi,Yi,Wi，分别代表目标的 x 坐标，y 坐标和价值，数据用空格隔开。

输出格式

输出一个正整数，代表一颗炸弹最多能炸掉地图上目标的总价值数目。

数据范围

$0≤R≤10^9 $ 
$0<N≤10000$,  
$0≤X_i,Y_i≤5000$ 
$0≤W_i≤1000$

输入样例：

```
2 1
0 0 1
1 1 1
```

输出样例：

```
1
```

思路：

二维前缀和。

code

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 5010;
// 不用开两个数组，开一个先取值，后+=更新就行
int s[N][N];

int main()
{
    int n, R;
    cin >> n >> R;
    // 当大于5001时，全覆盖即可。
    R = min(5001, R);

    while( n -- )
    {
        int x, y, w;
        cin >> x >> y >> w;
        // 由于边缘线上的点无法被摧毁，为了方便聚焦中心，将整个图向右下移动一格
        // 或者也可以理解为R * R的区域覆盖(R - 1) * (R - 1)个格子
        x ++ , y ++;
        // 注意，多个目标可能在同一个点上，因此要累加。
        s[x][y] += w;
    }

    for(int i = 1; i <= 5001; i ++)
        for(int j = 1; j <= 5001; j ++)
            s[i][j] += s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];

    // 按照R矩形遍历
    int res = 0;
    for(int i = R; i <= 5001; i ++ )
        for(int j = R ; j <= 5001; j ++)
            // 遍历寻找res的最大值
            res = max(res, s[i][j] - s[i - R][j] - s[i][j - R] + s[i - R][j - R]);

    cout << res;
    return 0;
}
```
