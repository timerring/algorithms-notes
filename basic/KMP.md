## KMP

KMP算法，又称模式匹配算法，能够在线性时间内判定字符串 A[1\~N]是否为字符串B[1\~M]的子串，并求出字符串A在字符串B中各次出现的位置。

例题：

给定一个字符串 S，以及一个模式串 P，所有字符串中只包含大小写英文字母以及阿拉伯数字。

模式串 P 在字符串 S 中多次作为子串出现。

求出模式串 P 在字符串 S 中所有出现的位置的起始下标。

**输入格式**

第一行输入整数 N，表示字符串 P 的长度。

第二行输入字符串 P。

第三行输入整数 M，表示字符串 S 的长度。

第四行输入字符串 S。

**输出格式**

共一行，输出所有出现位置的起始下标（下标从 0 开始计数），整数之间用空格隔开。

**数据范围**

$1≤N≤10^5$
$1≤M≤10^6$

**输入样例**：

```
3
aba
5
ababa
```

**输出样例**：

```
0 2
```

### 最朴素的做法(暴力做法)

```cpp
S[N]-- source串(长), P[N]---pattern串(短);

for(int i = 0; i < n; i++)
{
    bool flag = true;
    for(int j = 0; j < m; j++)
    	if(S[i + j - 1] != P[j])
        {
            flag = false;
            break;
        }
}
```

### KMP算法

首先用一个例子模拟KMP过程。

原数组为s，匹配数组为p。

首先求解next数组。**next记录的就是当前作为后缀末位的j对应的前缀末位的位置。**总结来说next[i] 就是使子串 s[0…i] 有最长 相等前后缀 的前缀的最后一位的下标。（next[i] 表示使字串 s[0…i] 中前缀 s[0…k] 等于后缀 s[i-k…i] 的最大的 k。如果找不到相等的前后缀，就令 next[i] = -1。显然，**next[i] 就是所求最长相等 前后缀中 前缀的最后一位的下标**。）

```
例如：abababaab
next[0] = -1
next[1] = -1
next[2] = 0
next[3] = 1
next[4] = 2
next[5] = 3
next[6] = 4
next[7] = 0
next[8] = 1
```

同时，next数组不能是本身，因为如果是本身，则通过next数组跳过该整个字符串，从最开始重新比较就没有意义了。

> 通常使用next[N]可能会和头文件中冲突，因此采用ne[N]记录。求next的过程类似于一个自己和自己匹配的过程。

```cpp
// p[0...0] 的区间内一定没有相等前后缀
ne[0] = -1;
// 构造模板串的 next 数组 从1开始即可
for (int i = 1, j = -1; i < n; i ++)
{
    // 若前后缀匹配不成功，反复令 j 回退，直至到 -1 或是 s[i] == s[j + 1]
    while (j != -1 && p[i] != p[j + 1]) j = ne[j];
    if (p[i] == p[j + 1]) j ++; // 匹配成功时，最长相等前后缀变长，最长相等前后缀最后一位变大
    ne[i] = j; // 令 ne[i] = j，以方便计算 next[i + 1]
}
```

接下来进入 kmp，kmp 算法照葫芦画瓢，给定一个文本串 text 和一个模式串 pattern，然后判断模式串 pattern 是否是文本串 text 的子串。

以 text = “abababaabc”、pattern = “ababaab” 为例。令 i 指向 text 的当前欲比较位，令 j 指向 pattern 中当前已被匹配的最后位，这样只要 text[i] == pattern[j + 1] 成立，就说明 pattern[j + 1] 也被成功匹配，此时让 i、j 加 1 继续比较，直到 j 达到 m - 1(m 为 pattern 长度) 时说明 pattern 是 text 的子串。在这个例子中，i 指向 text[4]、j 指向 pattern[3]，表明 pattern[0…3] 已经全部匹配成功了，此时发现 text[i] == pattern[j + 1] 成立，这说明 pattern[4] 成功匹配，于是令 i、j 加 1。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/175110_818d6c13bf-ac831f.jpg)

**一直比较的是s[i]与p[j+1]是否匹配**。

当text[5] != pattern[4 + 1]，匹配失败。为了不让 j 直接回退到 -1，应寻求回退到一个离当前的 j （此时 j 为 4）最近的 j’，使得 text[i] == pattern[j’ + 1] 能够成立，并且 pattern[0…j’] 仍然与 text 的相应位置处于匹配状态，即 pattern[0…j’] 是 pattern[0…j] 的后缀。这很容易令人想到之前的求 next 数组时碰到的类似问题。答案是 pattern[0…j’] 是 pattern[0…j] 的最长相等前后缀。也就是说，只需要不断令 **j = next[j]**，直到 j 回退到 -1 或者是 text[i] == pattern[j + 1] 成立，然后继续匹配即可。**next 数组的含义就是当 j + 1 位失配时，j 应该回退到的位置。**对于刚刚的例子，当 text[5] 与 pattern[4 + 1] 失配时，令 j = next[4] = 2，然后我们会发现 text[i] == pattern[j + 1] 能够成立，因此就让它继续匹配，直到 j == 6 也匹配成功，这就意味着 pattern 是 text 的子串。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20230605104225253.png)

KMP算法过程：

```cpp
for (int i = 0, j = -1; i < m; i ++)
{
    while (j != -1 && s[i] != p[j + 1]) j = ne[j]; // 匹配不成功
    if (s[i] == p[j + 1]) j ++; // 匹配成功时，模板串指向下一位
    if (j == n - 1) // 模板串匹配完成，第一个匹配字符下标为 0，故到 n - 1
    {
        // 匹配成功时，文本串结束位置减去模式串结束位置即为起始位置（可以通过上图得知）
        cout << i - j << ' ';
        // pattern串在source串中出现的位置可能是重叠的，
        // 需要让 j 回退到一定位置，再让 i 加 1 继续进行比较
        // 回退到 ne[j] 可以保证 j 最大，即已经成功匹配的部分最长
        // 自己模拟一下：source:acababab和pattern:acab或者source:abababab和pattern:abab即可理解
        j = ne[j]; 
    }
}
```

**记住next数组永远是针对j，即pattern串操作的。**

**时间复杂度：**

时间复杂度是O(n+m)，这里while最多执行m次，j最多加m次。总共执行次数是O(2m)次。

整个 for 循环中  $\mathrm{i}$  是不断加1的，所以在整个过程中  $\mathrm{i}$  的变化次数是O(m) 级别，接下来考虑  $\mathrm{j}$  的变化，我们注意到  $\mathrm{j}$  只会在一行中增加，并且每次只加1，这样在整个过程中  $\mathrm{j}$  最多增加m次；而其它情况  $\mathrm{j}$  都是不断减小的，由于  $\mathrm{j}$  最小不会小于 -1 ，因此在整个过程中, j  最多只能减少  m  次。也就是说 while 循环对整个过程来说最多只会执行  $\mathrm{m}$  次，因此  $\mathrm{j}$  在整个过程中变化次数是 O(m)  级别的。由于  $\mathrm{i}$  和  $\mathrm{j}$  在整个过程中的变化次数都是  O(m)  ，因此 for 循环部分的整体复杂度就是  O(m)  。计算 next 数组需要  O(n)  的时间复杂度(分析方法上同)，因此 kmp 算法总共需要  O(n+m)  的时间复杂度。

### code

```cpp
#include <iostream>

using namespace std;

const int N = 1000010;
char p[N], s[N]; // 用 p 来匹配 s
// “next” 数组，若第 i 位存储值为 k
// 说明 p[0...i] 内最长相等前后缀的前缀的最后一位下标为 k
// 即 p[0...k] == p[i-k...i]
int ne[N]; 
int n, m; // n 是模板串长度 m 是模式串长度

int main()
{
    cin >> n >> p >> m >> s;

    // 自己不能是自己的前后缀
    ne[0] = -1;

    // 构造pattern串的 next 数组
    for (int i = 1, j = -1; i < n; i ++)
    {
        // 若前后缀匹配不成功，反复令 j 回退，直至到 -1 或是 s[i] == s[j + 1]
        while (j != -1 && p[i] != p[j + 1]) j = ne[j];
        if (p[i] == p[j + 1]) j ++; // 匹配成功时，最长相等前后缀变长，最长相等前后缀最后一位变大
        ne[i] = j; // 令 ne[i] = j，以方便计算 next[i + 1]
    }
    // KMP
    for (int i = 0, j = -1; i < m; i ++)
    {
       while (j != -1 && s[i] != p[j + 1]) j = ne[j]; //匹配失败，回溯
       if (s[i] == p[j + 1]) j ++; // 匹配成功
       if (j == n - 1) // n是pattern串的长度
       {
           // 匹配成功时，文本串结束位置减去模式串长度即为起始位置
           cout << i - j << ' ';
           j = ne[j]; 
       }
    }
   return 0;
}
```

参考文章：https://www.acwing.com/solution/content/129372/
