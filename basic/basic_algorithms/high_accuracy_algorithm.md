- [高精度算法详解](#高精度算法详解)
  - [高精度加法](#高精度加法)
    - [大整数的存储](#大整数的存储)
    - [计算过程](#计算过程)
    - [例题：高精度加法](#例题高精度加法)
    - [算法模板](#算法模板)
  - [高精度减法](#高精度减法)
    - [计算过程](#计算过程-1)
    - [例题：高精度减法](#例题高精度减法)
    - [算法模板](#算法模板-1)
  - [高精度乘法](#高精度乘法)
    - [计算过程](#计算过程-2)
    - [例题：高精度乘法](#例题高精度乘法)
    - [算法模板](#算法模板-2)
  - [高精度除法](#高精度除法)
    - [计算过程](#计算过程-3)
    - [例题：高精度除法](#例题高精度除法)
    - [算法模板](#算法模板-3)


# 高精度算法详解

## 高精度加法

适用于c++。java和python没有这个问题，因为java有大整数类，python自带，默认数是无限大。

分类：

+ A + B   数量级$10^6$ （A和B的位数是 $10^6$ ）

+ A - B    数量级$10^6$

+ A * a   数量级$A ≤ 10^6$，$a ≤ 10000$

+ A / a    数量级$A ≤ 10^6$，$a ≤ 10000$

### 大整数的存储

实际上是把长数字的每一位存储到数组当中，并且是倒序存储。

![image-20220911114231825](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911114231825.png)

倒序存储的原因：

在数组中，如果顺序存储，在产生进位时，在数组头部a[0]前添加元素非常不方便，需要将后面元素依次后移。而如果倒序存储，则直接可以添加进位数字。

### 计算过程

先看一下我们如何进行常见加法：

![image-20220911115327631](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911115327631.png)

接下来我们将它进行抽象

![image-20220911115352984](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911115352984.png)

针对其中的一位，我们列以下式子：

![image-20220911115939270](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911115939270.png)

### 例题：高精度加法

给定两个正整数（不含前导 0），计算它们的和。

**输入格式**

共两行，每行包含一个整数。

**输出格式**

共一行，包含所求的和。

**数据范围**

$1≤整数长度≤100000$

```
12
23
```

**输出样例：**

```
35
```

### 算法模板

```cpp
#include <iostream>
#include <vector>

using namespace std;
//函数是算数组表示的整数A和数组表示的整数B
vector<int> add(vector<int> &A, vector<int> &B)
{
    if (A.size() < B.size()) return add(B, A);

    vector<int> C;
    int t = 0;// 进位
    for (int i = 0; i < A.size(); i ++ )
    {
        t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }

    //最后看最高位有没有进位，若有进位的话就压入
    if (t) C.push_back(t);
    return C;
}

int main()
{
    string a, b;
    vector<int> A, B;
    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i -- ) B.push_back(b[i] - '0');

    auto C = add(A, B);

    // 整个过程都是逆序存储的，因此最后输出时也需要倒序输出。
    for (int i = C.size() - 1; i >= 0; i -- ) cout << C[i];
    cout << endl;

    return 0;
}
```

模板说明

该模板采用了递归调用

```cpp
vector<int> add(vector<int> &A, vector<int> &B)
{
    if (A.size() < B.size()) return add(B, A);

    vector<int> C;
    int t = 0;//进位
    for (int i = 0; i < A.size(); i ++ )
    {
        t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    //最后看最高位有没有1，若是1的话就压入
    if (t) C.push_back(t);
    return C;
}
```

此模板也可以换种写法

```cpp
vector<int> add(vector<int> &A, vector<int> &B)
{

    vector<int> C;
    int t = 0;//进位
    for (int i = 0; i < A.size() || i < B.size(); i ++ )
    {
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    //最后看最高位有没有1，若是1的话就压入
    if (t) C.push_back(t);
    return C;
}
```

## 高精度减法

整数的存储同上

### 计算过程

这里以下式为例

![image-20220911132646580](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911132646580.png)

再次把它抽象一下，形成一下形式

![image-20220911133800828](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911133800828.png)

由此我们可以列出以下式子

首先判断A 与 B的关系，如果A 大于B，则正常加减，否则就计算其差的负数。

![image-20220911134113852](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911134113852.png)

然后分别判断每一位的大小，并且计算是否需要进位。（减去下一位的借位，没借位则t是0，有借位则t是1）

如果是大于0的话，就直接减，如果是小于0的话，就借一位再减。

![image-20220911134506979](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911134506979.png)

### 例题：高精度减法

给定两个正整数（不含前导 0），计算它们的差，计算结果可能为负数。

**输入格式**

共两行，每行包含一个整数。

**输出格式**

共一行，包含所求的差。

**数据范围**

1≤整数长度≤$10^5$

**输入样例：**

```
32
11
```

**输出样例：**

```
21
```

### 算法模板

```cpp
#include <iostream>
#include <vector>

using namespace std;

//判断是否有A ≥ B
bool cmp(vector<int> &A, vector<int> &B)
{
    //首先判断两个数的位数大小，设置return A > B
    if (A.size() != B.size()) return A.size() > B.size();
    // 位数相同，从最高位开始比较，设置return A > B
    for (int i = A.size() - 1; i >= 0; i -- )
        if (A[i] != B[i])
            return A[i] > B[i];
    return true;
}

vector<int> sub(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i ++ )
    {
        // 倒序存储，因此实际上是从低位逐渐向高位遍历。
        t = A[i] - t;
        // 首先要判断以下B[i]是否存在
        if (i < B.size()) t -= B[i];
        // 本位，这里的(t + 10) % 10,如果t是0-9，则会抵消，如果t是小于0，则相当于是 +10 借位。
        C.push_back((t + 10) % 10);
        // t小于0，表示需要借位，因此标记为1，这样在传递到下一次循环（即前一位时会自动减1（减去借位））
        if (t < 0) t = 1;
        else t = 0;
    }
    //删除前导0
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

int main()
{
    string a, b;
    vector<int> A, B;
    cin >> a >> b;
    for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');
    for (int i = b.size() - 1; i >= 0; i -- ) B.push_back(b[i] - '0');

    vector<int> C;

    if (cmp(A, B)) C = sub(A, B);
    else C = sub(B, A), cout << '-';

    for (int i = C.size() - 1; i >= 0; i -- ) cout << C[i];
    cout << endl;

    return 0;
}
```

## 高精度乘法

存储与上相同。

### 计算过程

这里先列出计算式的通式

![image-20220911154058993](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911154058993.png)

与常见的计算相似，每一位分别考虑进位和取余当前的数字

当前位：$C_0 = (A_0 * B_1 B_0 + t) \% 10$

进位：$t = (A_0 * B_1 B_0) / 10$

注意：这里是把B看成一个整体，而不是和一般的乘法一样。这样b方便计算，同时也方便存储（直接存为int就行）。

### 例题：高精度乘法

给定两个非负整数（不含前导 0） A 和 B，请你计算 A×B 的值。

**输入格式**

共两行，第一行包含整数 A，第二行包含整数 B。

**输出格式**

共一行，包含 A×B 的值。

**数据范围**

$1≤A的长度≤100000$,  
$0≤B≤10000$

**输入样例：**

```
2
3
```

**输出样例：**

```
6
```

### 算法模板

```cpp
#include <iostream>
#include <vector>

using namespace std;


vector<int> mul(vector<int> &A, int b)
{
    vector<int> C;

    //进位
    int t = 0;
    for (int i = 0; i < A.size() || t; i ++ )
    {
        if (i < A.size()) t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }

    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}


int main()
{
    string a;
    int b;

    cin >> a >> b;

    vector<int> A;
    for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');

    auto C = mul(A, b);

    for (int i = C.size() - 1; i >= 0; i -- ) printf("%d", C[i]);

    return 0;
}
```

核心模板

```cpp
if (i < A.size()) t += A[i] * b;
C.push_back(t % 10);
t /= 10; 
```

## 高精度除法

### 计算过程

高精度除法的通式如下：

![image-20220911165652347](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220911165652347.png)

仿照求解除法的过程，可以设计高精度除法算法如下：

最开始余数r为0

（核心模板）

+ $r = r *10 + A _3$

+ $C_3 = (r *10 + A _3) / b$ ;

+ $r = r \% b$

### 例题：高精度除法

给定两个非负整数（不含前导 0） A，B，请你计算 A/B 的商和余数。

**输入格式**

共两行，第一行包含整数 A，第二行包含整数 B。

**输出格式**

共两行，第一行输出所求的商，第二行输出所求余数。

**数据范围**

$1≤A的长度≤100000$
$1≤B≤10000$
B 一定不为 0

**输入样例：**

```
7
2
```

**输出样例：**

```
3
1
```

### 算法模板

```cpp
#include<bits/stdc++.h>
using namespace std;

vector<int> div(vector<int> &A, int b, int &r)
{
    vector<int> C;
    r = 0;
    //这里r为余数

    for(int i = A.size() - 1; i >= 0; i--)
    {
        //余数*10加下一位，输出本位数字为r/b，接着继续取余r
        r = r *10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    //由于除法存储是由高到低顺序的，但是通用输出是由低位到高位逆序的，因此这里reverse一下。
    reverse(C.begin(), C.end());

    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}


int main()
{
    string a;
    int b;

    cin >> a >> b;
    vector<int> A;
    for(int i = a.size() - 1; i >= 0; i--) A.push_back(a[i] - '0');

    int r;
    auto C = div(A, b,r);
    for(int i = C.size() -1 ; i >= 0; i--)printf("%d",C[i]);

    cout << endl << r << endl;
    return 0;

}
```
