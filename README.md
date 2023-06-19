# algorithms-notes
Algorithm for Interview and Operating Examination

> If you find it useful, welcome to star⭐, and you are also welcome to submit an issue for further discussion or PR proofreading.

算法markdown笔记，对于其中的一些冗余部分做了精简，大量的原创原理图辅助理解，方便阅读与记忆。

如果觉得有用，欢迎star⭐，同时也欢迎提issue进一步讨论或pr校对。


### Overview

| Title                                                        | Content                                                      | Example                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Basic Algorithms**                                         |                                                              |                                                              |
| 快速排序                                                     |                                                              |                                                              |
| 归并排序                                                     |                                                              |                                                              |
| [二分查找](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/binary_search.md#二分查找) | [整数二分](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/binary_search.md#整数二分)   \|   [二分步骤](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/binary_search.md#二分步骤)   \|   [浮点数二分](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/binary_search.md#浮点数二分)   \|   [二分模板整理](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/binary_search.md#二分模板整理) | [例题：数的范围](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/binary_search.md#例题数的范围)   \|   [例题：开平方](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/binary_search.md#例题开平方)   \|   [练习：数的三次方根](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/binary_search.md#练习数的三次方根)    \|    [练习：剑指 Offer II 072. 求平方根](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/binary_search.md#练习剑指-offer-ii-072-求平方根) |
| [高精度算法详解](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/high_accuracy_algorithm.md#高精度算法详解) | [高精度加法](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/high_accuracy_algorithm.md#高精度加法)   \|     [ 高精度减法](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/high_accuracy_algorithm.md#高精度减法)   \|   [高精度乘法](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/high_accuracy_algorithm.md#高精度乘法)   \|   [高精度除法](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/high_accuracy_algorithm.md#高精度除法) | [例题：高精度加法](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/high_accuracy_algorithm.md#例题高精度加法)   \|   [例题：高精度减法](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/high_accuracy_algorithm.md#例题高精度减法)   \|   [例题：高精度乘法](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/high_accuracy_algorithm.md#例题高精度乘法)   \|   [例题：高精度除法](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/high_accuracy_algorithm.md#例题高精度除法) |
| 前缀和                                                       |                                                              |                                                              |
| [差分算法及模板应用](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/difference_algorithm.md#差分算法及模板应用) | [一维差分](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/difference_algorithm.md#一维差分)   \|   [二维差分](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/difference_algorithm.md#二维差分) | [例题：差分](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/difference_algorithm.md#例题差分)   \|   [例题：差分矩阵](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/difference_algorithm.md#例题差分矩阵) |
| [双指针算法](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/double_pointer.md#双指针算法) | [基本思路](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/double_pointer.md#基本思路采用双指针算法)    \|  [模板应用](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/double_pointer.md#模板应用) | [最长连续不重复子序列](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/double_pointer.md#最长连续不重复子序列)   \|   [数组元素的目标和](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/double_pointer.md#数组元素的目标和)    \|   [判断子序列](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/double_pointer.md#判断子序列) |
| [位运算](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/bitwise_operation.md#位运算) | [lowbit(x)](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/bitwise_operation.md#lowbitx返回x的最后一位1)    \|   [位运算模板](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/bitwise_operation.md#位运算模板) | [例题：二进制中1的个数](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/bitwise_operation.md#例题二进制中1的个数) |
| [离散化及模板详解](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/discretization.md#离散化及模板详解) | [基本思想](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/discretization.md#基本思想)    \|    [算法思路](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/discretization.md#算法思路)    \|   [模板](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/discretization.md#模板) | [例题：区间和](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/discretization.md#例题区间和) |
| [区间合并算法及模板应用](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/interval_merge.md#区间合并算法及模板应用) | [基本思想](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/interval_merge.md#基本思想)   \|   [算法思路](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/interval_merge.md#算法思路) | [例题：区间合并](https://github.com/timerring/algorithms-notes/blob/main/basic/basic_algorithms/interval_merge.md#例题区间合并) |
| **Data Structure**                                           |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |





### 参考书籍

### ChangeLog

- v1.1 第一章内容更新 230618
- v1.0 基础结构 230618

### TODO

- 更新预计每日一节
- 欢迎issue交流讨论，PR订正。

## 关注更多

<div align=center>
<p>扫描下方二维码关注公众号：AIShareLab</p>
<img src="resources/qrcode.jpg" width = "180" height = "180">
</div>


&emsp;&emsp;AIShareLab，一个关注CV、AI、区块链、Web开发、硬件开发、5G通信等领域的热“AI”分享的社群，微信搜索公众号 AIShareLab 一起交流更多相关知识，前沿算法，Paper解读，项目源码，面经总结。﻿

## LICENSE

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/license-CC%20BY--NC--SA%204.0-lightgrey" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议</a>进行许可。
