- [二分查找](#二分查找)
  - [整数二分](#整数二分)
  - [二分步骤](#二分步骤)
    - [例题：数的范围](#例题数的范围)
    - [代码模板](#代码模板)
  - [浮点数二分](#浮点数二分)
    - [例题：开平方](#例题开平方)
      - [代码模板](#代码模板-1)
      - [总结](#总结)
    - [练习：数的三次方根](#练习数的三次方根)
      - [code](#code)
      - [总结](#总结-1)
    - [练习：剑指 Offer II 072. 求平方根](#练习剑指-offer-ii-072-求平方根)
      - [code](#code-1)
      - [总结](#总结-2)
  - [二分模板整理](#二分模板整理)


# 二分查找

## 整数二分

如果有单调性，就一定可以二分。但是有二分的不一定非得有单调性。

二分的本质是边界，将区间分为两个，一边满足某条性质，另一边不满足某条性质。然后可以找到这两个区间的边界，找任意一个区间的边界都可以。

![image-20220910092809336](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220910092809336.png)

但是找红色边界和绿色边界略有区别：

红色边界：

![image-20220910093531580](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220910093531580.png)

细节：关于为什么mid = (l + r +1) / 2 ，因为C++中取整是下取整。

+ 假设mid = (l + r ) / 2 ；如果是 l = r - 1;那么下取整后 mid = l ，会陷入死循环。 

也可以找绿色边界：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220910093736045.png)

+ 这里找绿色边界就不需要，因为如果r = l + 1;那么下取整后 mid = l，而不会取到r，因此不会产生死循环。

## 二分步骤

基本思路

整数二分步骤

1.找一个区间[L，R]，使得答案一定在该区间中

2.找一个判断条件，使得该判断条件具有二段性，并且答案一定是该二段性的分界点。

3.分析终点M在该判断条件下是否成立，如果成立，考虑答案在哪个区间;如果不成立，考虑答案在哪个区间;

4.如果更新方式写的是R = Mid，则不用做任何处理;如果更新方式写的是L= Mid，则需要在计算Mid时加上1。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/27-15-45-58-1679903145.png)

第一类：ans是红色区间的右端点

因此将[L, R] 分成 [L, M-1] [M, R]。

if M是红色的，说明ans仍然在[M, R]

else 说明ans必然在[L, M - 1]。

```cpp
while(L < R)
{
    M = (L + R + 1) / 2;
    if M red : L = M;
    else R = M - 1;
}
```

第二类：ans是绿色区间的左端点。

因此将[L, R] 分成[L, M] [M + 1, R]

if M是绿色的，说明ans仍然在[L, M]

else 说明ans必然在[M + 1, R]。

```cpp
while(L < R)
{
    M = (L + R) / 2;
    if M green : R = M;
    else L = M + 1;
}
```

注意区分M的取值，一般`L = M`时需要令 `M = (L + R + 1) / 2` ，因为cpp是向下取整，因此如果还是`M = (L + R) / 2` 的话，如果此时 L = R - 1,那么此时计算M = L（下取整），又由L = M，可知陷入死循环。

R = M 时就没有此限制，当R = L - 1时，经过计算仍然下取整 M = L - 1，结束计算。

### 例题：数的范围

给定一个按照升序排列的长度为 n 的整数数组，以及 q 个查询。

对于每个查询，返回一个元素 k 的起始位置和终止位置（位置从 0 开始计数）。

如果数组中不存在该元素，则返回 `-1 -1`。

**输入格式**

第一行包含整数 n 和 q，表示数组长度和询问个数。

第二行包含 n 个整数（均在 1∼10000 范围内），表示完整数组。

接下来 q 行，每行包含一个整数 k，表示一个询问元素。

**输出格式**

共 q 行，每行包含两个整数，表示所求元素的起始位置和终止位置。

如果数组中不存在该元素，则返回 `-1 -1`。

**数据范围**

$1≤n≤100000$
$1≤q≤10000$
$1≤k≤10000$

**输入样例：**

```
6 3
1 2 2 3 3 4
3
4
5
```

**输出样例：**

```
3 4
5 5
-1 -1
```

### 代码模板

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 100010;
int m ,n ;
int q[N];


int main()
{
    scanf("%d%d", &n, &m);
    for(int i = 0; i < n ; i++)scanf("%d",&q[i]);

    while( m --)
    {
        int x; 
        scanf("%d", &x);

        int l = 0 , r = n - 1;
        while( l < r)
        {
            int mid = l + r >> 1;
            // mid是否满足右侧性质，如果满足则r = mid，缩小范围。
            if(q[mid] >= x) r = mid;
            // 直到找到不满足的，让l = mid + 1，即小于x的数中最大的。
            else l = mid + 1;
        }
        // 上面二分出来的是第一个满足大于等于x的数，如果没有x，则是大于x的数。即左边界
        if(q[l] != x)cout << "-1 -1" <<endl;
        // 对该数进行判断，如果不满足，则返回-1-1。
        else
        {
            // 找到最后一个x的位置
            cout << l << ' ';

            int l = 0, r = n - 1;
            // 查找右边界
            while(l < r)
            {
                int mid = l + r + 1 >> 1;
                // 这里可以看到，判断的依据就是mid是否满足左侧的性质，满足则至少让l = mid。
                if(q[mid] <= x) l = mid;
                // 如果不满足说明越过了，这时将右边界缩小到原mid内。
                else r = mid - 1;
            }
            cout << l << endl;
        }
    }
}
```

## 浮点数二分

浮点数二分思路同上，有个好处是不需要处理边界。

### 例题：开平方

给定一个浮点数 n，求它的平方根。

**输入格式**

共一行，包含一个浮点数 n。

**输出格式**

共一行，包含一个浮点数，表示问题的解。

注意，结果保留 6 位小数。

**数据范围**

−10000≤n≤10000

**输入样例：**

```
4
```

**输出样例：**

```
2.000000
```

#### 代码模板

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220910120045386.png)

```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    double x;
    cin >> x;

    double l = 0, r = max(1,x);
    while(r - l > 1e-8)
    {
        double mid = (l + r)/2;
        if(mid * mid >= x)r = mid;
        else l = mid ;
    }
    printf("%lf", l);
    return 0;
}
```

#### 总结

这里要强调的是精度问题：

```cpp
while(r - l > 1e-8)
```

误差过大会导致精度不足。

这里给出一些经验值：误差值一般比保留位数多2

| 保留位数 | 误差值  |
| ---- | ---- |
| 4    | 1e-6 |
| 5    | 1e-7 |
| 6    | 1e-8 |

当然可以采用其他写法：

```
for(int i = 0; i < 100 ; i++);
```

直接循环100次，相当于把整个区间的长度直接循环$2^{100}$.

此外，还需要注意，当 x < 1时，例如x = 0.01，则开根号x = 0.1,因此区间不能取[0,x]，建议取[0, max(1,x)]。

### 练习：数的三次方根

给定一个浮点数 n，求它的三次方根。

**输入格式**

共一行，包含一个浮点数 n。

**输出格式**

共一行，包含一个浮点数，表示问题的解。

注意，结果保留 6 位小数。

**数据范围**

−10000≤n≤10000

**输入样例：**

```
1000.00
```

**输出样例：**

```
10.000000
```

#### code

```cpp
# include<iostream>
using namespace std;

int main(){
    double x;
    cin >> x;

    double l = -10000, r = 10000;
    while( r - l > 1e-8)
    {
        double mid = (r + l)/2;
        if(mid * mid * mid >= x) r = mid;
        else l = mid;
    }
    printf("%lf", l);
    return 0;
}
```

#### 总结

这里还是需要注意以下边界问题，区间还不能写为-x到x，因为对于绝对值小于1的数，其三次方根大于1，导致该区间内找不到对应的根。

### 练习：剑指 Offer II 072. 求平方根

[剑指 Offer II 072. 求平方根](https://leetcode.cn/problems/jJ0w9p/)

给定一个非负整数 x ，计算并返回 x 的平方根，即实现 int sqrt(int x) 函数。

正数的平方根有两个，只输出其中的正数平方根。

如果平方根不是整数，输出只保留整数的部分，小数部分将被舍去。

示例 1:

```
输入: x = 4
输出: 2
```

示例 2:

```
输入: x = 8
输出: 2
解释: 8 的平方根是 2.82842...，由于小数部分将被舍去，所以返回 2
```

#### code

```cpp
class Solution {
public:
    int mySqrt(int x) {
        int l = 0, r = x;
        while(r >= l){
            int mid = (r + l) >> 1;
            if((long long)mid * mid <= x) l = mid + 1;
            else r = mid - 1;
        }
        return l - 1;
    }
};
```

#### 总结

其中如果直接 l = mid，会导致死循环超时，这点很好理解，例如一个根是2.8，如果区间是 l = 2, r = 3。mid由于是整数，因此为2，此时mid小于2.8，如果这时还是l = mid，相当于l一直没动，因此会超时。但是如果l = mid + 1，这时l越界，终止，最后返回上一次的整数，即l - 1即可。

此外这道题还可以采用右边界的方法寻找平方根，本质上和上面是一个类似的过程。

```cpp
class Solution {
public:
    int mySqrt(int x) {
        int l = 0, r = x;
        while(r >= l){
            int mid = (r + l) >> 1;
            if((long long) mid * mid > x) r = mid - 1;
            else l = mid + 1;
        }
        return r;
    }
};
```





## 二分模板整理

```cpp
//查找左边界 SearchLeft 简写SL
int SL(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid; 
        else l = mid + 1; 
    }   
    return l;
}
//查找右边界 SearchRight 简写SR 
int SR(int l, int r) 
{
    while (l < r)
    {                   
        int mid = l + r + 1 >> 1; //需要+1 防止死循环
        if (check(mid)) l = mid;
        else r = mid - 1; 
    }
    return r; 
}

```


