## 双指针算法

双指针算法的常见情况：

+ 双指针在两个数组上（例如归并排序等等）
  
  ![image-20220927205326361](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220927205326361.png)

+ 双指针在一个数组上
  
  ![image-20220927205402668](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220927205402668.png)

常见通用代码模板

```cpp
for(i = 0, j =0; i < n; i++ )
{
    while(j < i && check(i,j))j++;
    //再加上每道题目的具体逻辑
}
```

双指针的核心思想是**优化**。

常见的遍历一共是双重循环，复杂度是O($n^2$)

但是双指针算法虽然是看起来是双重循环，但是实际上每个指针移动的次数是不超过O(n)的，两个指针的总次数不超过O(2n)。将之前的朴素算法优化到O(n)。

### 举例：分行输出字符串

假设有一个字符串“acb def jhi”以空格分开，现在要将其以空格为分解，换行输出。

#### 基本思路：采用双指针算法

首先i和j在同一起点位置，然后j进行扫描。

![image-20220927214239839](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220927214239839.png)

j停在空格分界的位置上，输出两位置之间的字符串

![image-20220927214331502](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220927214331502.png)

把指针i移动在j上。

![image-20220927214357634](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220927214357634.png)

#### 模板应用

```cpp
#include <iostream>
#include <string.h>

using namespace std;

int main()
{
    char str[1000];

    gets(str);

    int n = strlen(str);

    for(int i = 0; i< n; i++)
    {
        int j = i;
        while(j < n && str[j] != ' ')j ++;

        //具体逻辑
        for(int k = i; k < j;k++)cout << str[k];

        i = j;
    }
    return 0;
}
```

### 最长连续不重复子序列

给定一个长度为 n 的整数序列，请找出最长的不包含重复的数的连续区间，输出它的长度。

**输入格式**

第一行包含整数 n。

第二行包含 n 个整数（均在 0∼10^5范围内），表示整数序列。

**输出格式**

共一行，包含一个整数，表示最长的不包含重复的数的连续区间的长度。

**数据范围**

$1≤n≤10^5$

**输入样例：**

```
5
1 2 2 3 5
```

**输出样例：**

```
3
```

**朴素做法：**

```cpp
for(int i = 0; i < n; i ++)
{
    for(int j = 0; j < i; j++)
       if(chack(i,j))
       {
           res = max(res, i - j +1);
       }
}
```

#### 双指针算法模板：

```cpp
for(int i = 0; i < n; i++)
{
    while(j <= i && check(j,i))j ++;
    res = max(res, i - j + 1);
}
```

#### 双指针基本思路：

首先i循环遍历，j的含义是j最远能到什么地方，因为需要计算的是无重复的个数，因此j和i之间无重复的数。

> 可以证明：在i不断后移同时，j必然也是单调后移的，不可能出现j前移的情况，因为j如果前移，那么就证明刚刚最大的位置并非最优值，这与刚刚的结论矛盾。
> 
> ![image-20220927221907084](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220927221907084.png)

有了单调这一层性质，就可以采用双指针这种单调队列的思想优化。因为可以使j在i遍历的时候仍然记录上次的位置。

具体条件的应用；

+ 开辟一个动态数组来记录每个值出现多少次。例如原来需要判断的数组为a[n]。记录时就可以另外开辟以该值为序列号的数组S[N]; 
  + i往后移动一格，代表有一个数进来了，即S[a[i]]++;
  + j往后移动一格，代表有一个数出去了，即S[a[j]]--;

这样可以动态地统计区间内有多少个数。

+ 其中如果有重复的值，一定是新加进来的a[i]，那么那个值统计后，该记录数组的值大于1，那么j下次就必须去掉那个值，移动到该值之后。

> 这里如果j > i的时候，一定了要求，区间里一个数都没有了，就会不满足S[a[i]] > 1，因此本题这个比较条件j <= i可以不写。

```cpp
for(int i = 0; i < n; i++)
{
    while(S[a[i]] > 1)XXX;
    res = max(res, i - j + 1);
}
```

#### 代码

```cpp
#include <iostream>

using namespace std;

const int N = 100010;

int S[N],a[N];
int n;


int main()
{
    cin >> n;
    for(int i = 0; i< n; i++)cin >> a[i];

    int res = 0;
    for(int i = 0, j = 0; i < n; i++)
    {
        S[a[i]]++;
        while(S[a[i]] > 1)
        {
            //当停止时，说明和i相同的值已经被删了，即j停在了重复值之后
            S[a[j]] --;
            j ++;
        }

        res = max(res, i - j + 1);
    }
    cout << res;
    return 0;
}
```

当数组很大的时候，可以考虑采用哈希表来实现。哈希表可以存任意量，包括字母，数字，字符串。

注意：要想采用双指针算法优化，重要的是这一种**单调关系**。





### 数组元素的目标和

给定两个升序排序的有序数组 A 和 B，以及一个目标值 x。

数组下标从 0 开始。

请你求出满足 A[i]+B[j]=x 的数对 (i,j)。

数据保证有唯一解。

**输入格式**

第一行包含三个整数 n,m,x，分别表示 A 的长度，B 的长度以及目标值 x。

第二行包含 n 个整数，表示数组 A。

第三行包含 m 个整数，表示数组 B。

**输出格式**

共一行，包含两个整数 i 和 j。

**数据范围**

数组长度不超过 $10^5$。  
同一数组内元素各不相同。  
$1≤数组元素≤10^9$

**输入样例：**

```
4 5 6
1 2 4 7
3 4 6 8 9
```

**输出样例：**

```
1 1
```

通过观察可以发现，当a不断增大时，b是相应减小的，因此存在单调性，可以用双指针解决。因为j指针不会回退。

```cpp
#include <iostream>

using namespace std;
const int N = 1e5 + 10;
int a[N], b[N];

int n, m, x;

int main()
{
    cin >> n >> m >> x;
    for(int i = 0; i < n; i ++) scanf("%d", &a[i]);
    for(int i = 0; i < m; i ++) scanf("%d", &b[i]);
    
    // This level of monotonicity is critical
    // Algorithmic Complexity Bits: O(m + n)
    for(int i = 0, j = m - 1; i < n; i ++)
    {
        while(j >= 0 && a[i] + b[j] > x) j --;
        if(a[i] + b[j] == x) cout << i << " " << j;
    }
    
    
    return 0;
}
```





### 判断子序列

给定一个长度为 n 的整数序列 a1,a2,…,an 以及一个长度为 m 的整数序列 b1,b2,…,bm。

请你判断 a 序列是否为 b 序列的子序列。

子序列指序列的一部分项按**原有次序排列**而得的序列，例如序列 {a1,a3,a5} 是序列 {a1,a2,a3,a4,a5} 的一个子序列。

**输入格式**

第一行包含两个整数 n,m。

第二行包含 n 个整数，表示 a1,a2,…,an。

第三行包含 m 个整数，表示 b1,b2,…,bm。

**输出格式**

如果 a 序列是 b 序列的子序列，输出一行 `Yes`。

否则，输出 `No`。

**数据范围**

$1≤n≤m≤10^5$,  
$−10^9≤a_i,b_i≤10^9$

**输入样例：**

```
3 5
1 3 5
1 2 3 4 5
```

**输出样例：**

```
Yes
```

**思路：**

j指针用来扫描整个b数组，i指针用来扫描a数组。若发现a[i] == b[j]，则让i指针后移一位。
整个过程中，j指针不断后移，而i指针只有当匹配成功时才后移一位，若最后若i == n，则说明匹配成功。

**为什么双指针做法是正确的？**

整个过程中j指针不断扫描b数组并且向后移动，相当于不断给i指针所指向的a数组创建匹配的机会，只有匹配成功时i指针才会向后移动一位，当i == n时，说明全部匹配成功。同时这样做也保证了**顺序的正确性**。因为本来就是从左往右的。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/04/08-21-52-57-1680961976.png)

```cpp

#include<iostream>

using namespace std;
const int N = 1e5 + 10;
int n, m, a[N], b[N];

int main()
{
    cin >> n >> m;
    for(int i = 0; i < n; i ++) scanf("%d", &a[i]);
    for(int i = 0; i < m; i ++) scanf("%d", &b[i]);
    
    int i = 0, j = 0;
    while(i < n && j < m)
    {
        if(a[i] == b[j]) i ++;
        j ++;
    }
    
    if(i == n) puts("Yes");
    else puts("No");
    
    return 0;
}
```
