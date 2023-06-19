- [位运算](#位运算)
  - [lowbit(x):返回x的最后一位1。](#lowbitx返回x的最后一位1)
    - [例题：二进制中1的个数](#例题二进制中1的个数)
    - [code](#code)
- [位运算模板](#位运算模板)


# 位运算

位运算常用操作：查看整数n的二进制表示中第k位是几。

eg : $n = 15 = (1111)_2$

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220928224656481.png)

具体步骤：

1. 先把第k位数字移动到最后一位( n >> k )
2. 看个位是几 (x & 1)

> 注意：右移本身就是一个二进制层面的操作过程。在新版GNU C++中一般默认右移是算数右移。

将1和2结合起来，即：

```cpp
n >> k & 1;
```

例如：输出n的二进制表示

```cpp
#include <iostream>
#include <string.h>

using namespace std;

int main()
{
    int n = 10;
    for(int k = 3; k >= 0; k--)cout << (n >> k & 1);
    return 0;
}
```

## lowbit(x):返回x的最后一位1。

lowbit(n)定义为非负整数n在二进制表示下“最低位的1及其后边所有的0”构成的数值。如果定义为int，则返回的是一个十进制的数。

树状数组的基本操作。

应用：统计x里1的个数。每一次把x的最后一位1减掉(减去对应的数)，当最后x为0时，计算减的次数，就是x中含有1的个数。

eg: 

+ x = 1010   lowbit(x) return 10
+ x = 10110000   lowbit(x) return 10000

实际实现： `x & -x`

解释：由于是补码存储，因此 -x = ~x +1 ; 因此 x & -x = x &(~x + 1)

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220928225854477.png)

### 例题：二进制中1的个数

给定一个长度为 n 的数列，请你求出数列中每个数的二进制表示中 1 的个数。

**输入格式**

第一行包含整数 n。

第二行包含 n 个整数，表示整个数列。

**输出格式**

共一行，包含 n 个整数，其中的第 i 个数表示数列中的第 i 个数的二进制表示中 1 的个数。

**数据范围**

$1≤n≤100000$,
$0≤数列中元素的值≤10^9$

**输入样例**：

```
5
1 2 3 4 5
```

**输出样例**：

```
1 1 2 1 2
```

### code

```cpp
#include<iostream>
using namespace std;

int lowbit(int x)
{
    // 注意：返回是一个int十进制
    return x & -x;
}

int main()
{
    int n;
    cin >> n;

    while(n--)
    {
        int x, res = 0;
        cin >> x;

        // 每次减去x的最后一位1以及后面的部分
        while(x)x -= lowbit(x),res ++;

        cout << res <<' ';
    }
    return 0;
}
```



# 位运算模板

查看整数n的二进制表示中第k位是几

```cpp
n >> k & 1;
```

lowbit(x)

```cpp
int lowbit(int x)
{
    // 注意：返回是一个int十进制
    return x & -x;
}
```
