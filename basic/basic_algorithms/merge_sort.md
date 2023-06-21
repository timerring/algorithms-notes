- [归并排序](#归并排序)
  - [算法详解](#算法详解)
  - [例题：归并排序](#例题归并排序)
  - [算法模板](#算法模板)
  - [练习：逆序对](#练习逆序对)
  - [练习：剑指 Offer 51. 数组中的逆序对](#练习剑指-offer-51-数组中的逆序对)


## 归并排序

### 算法详解

稳定，思想：分治

时间复杂度：nlogn

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908204727373.png)

以数组的中间部分来分，分为左边和右边。

+ 确定分界点。mid = (1+r)/2;

+ 递归排序left和right。

+ 合二为一（重点）。合成一个有序的数组。

  合并的方法：双指针法。

  ![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20220908203821274.png)

  ![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/image-20220908203945084.png)

  ![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908204101457.png)

  由于归并排序是稳定的，因此在两数相同的时候可以把第一个数字移动到尾部。

  ![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908204332955.png)



### 例题：归并排序

给定你一个长度为 n 的整数数列。

请你使用归并排序对这个数列按照从小到大进行排序。

并将排好序的数列按顺序输出。

**输入格式**

输入共两行，第一行包含整数 $n$。

第二行包含 $n$ 个整数（所有整数均在 1∼$10^9$ 范围内），表示整个数列。

**输出格式**

输出共一行，包含 $n$ 个整数，表示排好序的数列。

**数据范围**

1≤ n ≤100000

**输入样例：**

```
5
3 1 2 4 5
```

**输出样例：**

```
1 2 3 4 5
```

### 算法模板

```cpp
#include <iostream>

using namespace std;

const int N = 1e5 + 10;

int a[N], tmp[N];

void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int mid = l + r >> 1;
    // 取中间的位置

    // 分别归并排序左右两侧，进行排序
    merge_sort(q, l, mid), merge_sort(q, mid + 1, r);

    // =========归并的过程===========
    // k表示已经归并的数，i为指向左半边序列的起点，j为指向右半边序列的起点。
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        // 进行判断，每次把小的那部分放在当前位置上。
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];
    // 如果左右两边没有循环完的话，贴在数组最后
    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];

    // 存回q数组中
    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}

int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);

    merge_sort(a, 0, n - 1);

    for (int i = 0; i < n; i ++ ) printf("%d ", a[i]);

    return 0;
}
```



### 练习：逆序对

给定一个长度为 n 的整数数列，请你计算数列中的逆序对的数量。

逆序对的定义如下：对于数列的第 i 个和第 j个元素，如果满足 i<j 且 a[i]>a[j]，则其为一个逆序对；否则不是。

输入格式

第一行包含整数 n，表示数列的长度。

第二行包含 n个整数，表示整个数列。

输出格式

输出一个整数，表示逆序对的个数。

数据范围

1≤n≤100000，
数列中的元素的取值范围 $[1,10^9]$。

输入样例：

```
6
2 3 4 5 6 1
```

输出样例：

```
5
```

code：

思路还是标准的归并，但是需要注意的是res的范围。首先不妨考虑最复杂的情况，即这组数是从大到小排序好的，也就是一共可以组成$(n - 1) + (n - 2) + ... + 1 = \frac{n (n - 1)}{2}$，这里n最大是100000，因此逆序对最大为$5*10^9$，超过了int范围，可以选择开long long存储。

```cpp
#include <iostream>

using namespace std;

typedef long long LL;

const int N = 1e5 + 10;

int a[N], tmp[N];

LL merge_sort(int q[], int l, int r)
{
    if (l >= r) return 0;

    int mid = l + r >> 1;
    // 收集每次排序中的逆序对
    LL res = merge_sort(q, l, mid) + merge_sort(q, mid + 1, r);

    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        // i指向的是左半段，j指向的是右半段
        // 正常情况是左半段小于右半段
        if (q[i] <= q[j]) tmp[k ++] = q[i ++];
        // 否则说明是逆序，j和（i到mid）之间的所有数都组成逆序对，因为i之后的部分都大于i，同样也就大于j。
        else
        {
            res += mid - i + 1;
            tmp[k ++ ] = q[j ++ ];
        }
    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];

    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];

    return res;
}

int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d", &a[i]);

    cout << merge_sort(a, 0, n - 1) << endl;

    return 0;
}
```



### 练习：剑指 Offer 51. 数组中的逆序对

[剑指 Offer 51. 数组中的逆序对](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

示例 1:

```
输入: [7,5,6,4]
输出: 5
```


限制：

```
0 <= 数组长度 <= 50000
```

code

```cpp
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        vector<int> tmp(nums.size());
        return merge_sort(nums, tmp, 0, nums.size() - 1);
    }

    long long merge_sort(vector<int>& nums, vector<int>& tmp, int l, int r)
    {
        if( l >= r) return 0;
        
        int mid = l + r >> 1;

        long long res = merge_sort(nums, tmp, l, mid) + merge_sort(nums, tmp, mid + 1, r);

        int k = 0, i = l, j = mid + 1;

        while( i <= mid && j <= r){
            if(nums[i] <= nums[j]) tmp[k ++ ] = nums[i ++ ];
            else{
                res += mid - i + 1;
                tmp[k ++ ] = nums[j ++ ];
            } 
        }

        while(i <= mid) tmp[k ++ ] = nums[i ++ ];
        while(j <= r) tmp[k ++ ] = nums[j ++ ];

        for(i = l, j = 0; i <= r; i ++, j ++ ) nums[i] = tmp[j];

        return res;
    }
};
```



