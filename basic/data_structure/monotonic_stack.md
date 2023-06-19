- [单调栈模板](#单调栈模板)
  - [栈算法模板](#栈算法模板)
    - [例题：单调栈](#例题单调栈)
      - [基本思路](#基本思路)
      - [code](#code)


# 单调栈模板

栈：先进后出。

队列：先进先出。

> 数组模拟栈和队列相较于STL的好处在于速度快，虽然在实际编译的时候会有O2优化，使两者相差无几，但是在算法题中一般没有优化。

## 栈算法模板

```cpp
// 栈定义为stk[N]，tt表示栈顶，初始化为0
int stk[N], tt = 0;

// 向栈顶插入一个数
stk[ ++ tt] = x;

// 从栈顶弹出一个数
tt -- ;

// 栈顶的值
stk[tt];

// 判断栈是否为空
/*
	if(tt > 0) not empty
	else empty
*/
if (tt > 0)
{
	
}
```

**单调栈常用与给定一个数，寻找在这个序列中每一个数的左边离他最近的且比他小的数在什么地方。**

### 例题：单调栈

给定一个长度为 N 的整数数列，输出每个数左边第一个比它小的数，如果不存在则输出 −1。

**输入格式**

第一行包含整数 N，表示数列长度。

第二行包含 N 个整数，表示整数数列。

**输出格式**

共一行，包含 N 个整数，其中第 i 个数表示第 i 个数的左边第一个比它小的数，如果不存在则输出 −1。

**数据范围**

$1≤N≤10^5$
$1≤$数列中元素$≤10^9$

**输入样例：**

```
5
3 4 2 7 5
```

**输出样例：**

```
-1 3 -1 2 2
```

#### 基本思路

定义一个栈，分别读入数据，然后判断栈中的数字，如果栈顶的数字大于等于读入的x，则将该数出栈（把大于它的所有数剔除），直到栈顶数字小于该数，输出该数，然后将x存入栈顶。这样可以保证栈内的数字始终是一个线性的存储。即输入的x可以找到离它最近的比他小的数。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20221103135351449.png)

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/55289_7a61998ec0-20201211221031165.gif)

#### code

```cpp
# include <iostream>

using namespace std;

const int N = 100010;

int n;
int stk[N], tt;

int main()
{
    cin >> n;
    
    for(int i = 0; i < n; i++)
    {
        int x;
        cin >> x;
        while(tt && stk[tt] >= x) tt --; // 如果栈顶元素大于当前待入栈元素，则出栈
        if(tt) cout << stk[tt] << ' '; // 如果栈不空，则该栈顶元素就是左侧第一个比它小的元素
        else cout << -1 << ' '; // 如果栈空，则没有比该元素小的值，输出 -1
        // 再将该元素添加进去
        stk[ ++ tt] = x;
    }
    
    return 0;
}
```

虽然这个算法中有两个循环，但是实际针对第二层循环，每个数只会有压入和弹出两个操作，并没有涉及到遍历，因此时间消耗为2N，时间复杂度为O(N)。

同样也可以使用STL实现：

```cpp
#include<iostream>
#include<vector>
using namespace std;
int main() {
    int n,x;
    cin >> n;
    vector<int> t;
    while (n--) {
        cin >> x;
        while (t.size() > 0 and t.back() >= x) {
            t.pop_back();
        }
        if (t.size() == 0) {
            cout << -1 << ' ';
        }else {
            cout << t.back()<<' ';
        }
        t.push_back(x);
    }
    return 0;
}
```

