- [区间合并算法及模板应用](#区间合并算法及模板应用)
  - [基本思想](#基本思想)
  - [算法思路](#算法思路)
  - [例题：区间合并](#例题区间合并)
      - [code](#code)


# 区间合并算法及模板应用

## 基本思想

将多个区间进行合并，其中有交集的区间合为一个区间，没有交集的区间保留原状。注意，这里端点重合也算作一种交集区间。

算法的图解如下：

![](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221014210207531.png)

## 算法思路

首先按照区间的左端点进行排序。

然后维护一个最左侧的区间。设头节点为st，尾节点尾ed。

可能会有以下三种情况：

1.下一个区间在本区间中。

![image-20221014211603753](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221014211603753.png)

则将区间更新为两个区间的并集，将尾节点设置为两区间最大的节点即可。

<!-- more -->

2.下一个区间有交集

![image-20221014211708307](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221014211708307.png)

3.下一个区间没有交集

![image-20221014211758657](https://raw.githubusercontent.com/timerring/picgo/master/picbed/image-20221014211758657.png)

将该区间放到result中，并且将区间st，ed移动至下一个区间（维护的区间更新为下一个区间）。

## 例题：区间合并

给定 n 个区间 [$l_i,r_i$]，要求合并所有有交集的区间。

注意如果在端点处相交，也算有交集。

输出合并完成后的区间个数。

例如：[1,3] 和 [2,6] 可以合并为一个区间 [1,6]。

**输入格式**

第一行包含整数 n。

接下来 n 行，每行包含两个整数 l 和 r。

**输出格式**

共一行，包含一个整数，表示合并区间完成后的区间个数。

**数据范围**

1≤n≤100000
$−10^9$≤$l_i$≤$r_i$≤$10^9$

**输入样例**：

```
5
1 2
2 4
5 6
7 8
7 9
```

**输出样例**：

```
3
```

#### code

```cpp
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

typedef pair<int, int> PII;

vector<PII> segs;

void merge(vector<PII> &segs)
{
    vector<PII> res;
    // 默认先对左端点（第一个变量）进行排序，默认升序排列
    // 两个参数分别是起始地址以及结束地址（数组中最后一个元素的下一个地址）
    sort(segs.begin(), segs.end());
    // 定义在参数区间的最左端
    int st = -2e9, ed = -2e9;

    for(auto seg: segs)
    {
        // 两区间不相交
        if (ed < seg.first)
        {
            // 将该区间保存，并且维护下一个区间
            if(st != -2e9 )res.push_back({st, ed});

            st = seg.first, ed = seg.second;
        }
        // 相交，取最大的尾节点（取并集）
        else ed = max(ed, seg.second);
    }
    // 把最后的一段区间也保存
    if (st != -2e9)res.push_back({st, ed});
    // 输出结果，注意是以引用的方式传参的，因此直接赋值即可。
    segs = res;
}

int main()
{
    int n;
    cin >> n;
    while(n --)
    {
        int l, r;
        cin >> l >> r;
        // 保存参数
        segs.push_back({l, r});
    }
    // 合并
    merge(segs);
    // 返回个数即可
    cout << segs.size() << endl;
    return 0;

}
```
