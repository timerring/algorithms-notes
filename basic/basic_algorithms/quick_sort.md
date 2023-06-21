- [快速排序](#快速排序)
  - [算法详解](#算法详解)
  - [例题：快速排序](#例题快速排序)
  - [算法模板](#算法模板)
  - [练习：排序数组](#练习排序数组)


## 快速排序

### 算法详解

不稳定,基于分治思想。

期望复杂度：nlogn，最坏$n^2$

+ 确定分界点

  常用分界点：取左边界q[l],取中间值q[(1+r)/2],取右边界q[r],随机值。

+ 调整区间

  保证左边数都小于等于x，右边数大于等于x即可。

  ![image-20220908091511855](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908091511855.png)

+ 递归处理左右两端

  左边排好序，右边排好序。

重点是如何调整区间：

常见思路：

1. 双数组法（比较耗费空间）

   再开两个数组，分别是a[],b[].

   然后对于原数组q，如果q[i] ≤ x，则将x存入a[]中，否则存入b[]中。

   最后先将a[]存入q中，再将b[]存入数中。

2. 双指针法

   采用双指针的思想。设置哨兵x进行比较。

   ![image-20220908092916232](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908092916232.png)
   
   让i，j向中间移动，如果i ≤  x，则继续移动，否则等待交换，如果 j ≥ x ，则继续移动，否则等待交换。当i和j都等待交换的时候，交换ij，然后继续移动。直到i大于为止。
   
   ![image-20220908194838485](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908194838485.png)
   
   ![image-20220908194917720](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908194917720.png)

### 例题：快速排序

给定你一个长度为 n 的整数数列。

请你使用快速排序对这个数列按照从小到大进行排序。

并将排好序的数列按顺序输出。

**输入格式**

输入共两行，第一行包含整数 n。

第二行包含 n 个整数（所有整数均在 1∼$10^9$ 范围内），表示整个数列。

**输出格式**

输出共一行，包含 n 个整数，表示排好序的数列。

**数据范围**

1≤n≤100000

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
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6 +10;

int q[N];

void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;
    //如果数组中就一个数，直接返回

    int x = q[l], i = l - 1, j = r + 1;
    //随机取数，注意这里i要在左边界左边，j要在右边界右边，是因为采用dowhile循环，开场会先执行一次。
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
        //当两侧都停下后，交换位置即可。
    }
	//递归地调用函数，排序左右两边。同时注意这里可能存在问题。
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}

int main()
{
    int n;
    scanf("%d", &n);

    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);

    quick_sort(q, 0, n - 1);

    for (int i = 0; i < n; i ++ ) printf("%d ", q[i]);

    return 0;
}
```

对于模板中的

```cpp
    int x = q[l], i = l - 1, j = r + 1;
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
        //假设是[1,2] x = 1，多轮交换过后一直是[0,1]，死循环就出不来了。
    }
	同时注意这里可能存在问题。
    quick_sort(q, l, j);//如果这里是j的话，x就一定不能取到q[r]
	//quick_sort(q, l, i - 1);如果这里是i的话，x就一定不能取到q[l]
	//quick_sort(q, i, r);
    quick_sort(q, j + 1, r);
```

详解

![image-20220908202800418](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908202800418.png)

![image-20220908202837923](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908202837923.png)

![image-20220908203044254](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20220908203044254.png)



### 练习：排序数组

给你一个整数数组 `nums`，请你将该数组升序排列。

示例 1：

```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```

示例 2：

```
输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]
```

code

```cpp
class Solution {
public:
    void quick_sort(vector<int>& nums, int l, int r){
        if(l >= r) return;
        int x = nums[rand() % (r - l + 1) + l], i = l - 1, j = r + 1;
        while(i < j){
            
            do(i ++ ); while( nums[i] < x );
            do(j -- ); while( nums[j] > x );
            if( i < j ) swap(nums[i], nums[j]);
        }
        quick_sort(nums, l, j);
        quick_sort(nums, j + 1, r);
    }

    vector<int> sortArray(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        quick_sort(nums, l, r);
        return nums;
    }

};
```

这题测试样例有点特殊设计，`x = nums[rand() % (r - l + 1) + l]` 选主元必须是随机的，否则无论是选左边 x = nums[l] 还是右边x = nums[r]都会超时。
