---
categories: algorithm
layout: post
---

- table
{:toc}

# Meet In The Middle

考虑这样一个问题，我们有一个大小为N($N\leq 40$)的集合S，我们希望找到一个它的子集，使得子集的和在小于M($M\leq 10^{18}$)的前提下尽可能大。

像这种问题，如果M比较小，可以用动态规划解决，如果N比较小可以用暴力枚举解决。但是现在我们两种方式都用不了。

Meet In the middle是一种非常简单的思路，将集合分成大小尽可能接近的两个不相交子集A、B。之后我们容易发现结果要么是A的子集，要么是B的子集，要么是A的某个子集和B的某个子集的并。

我们直接暴力解决是单个集合子集的情况，时间复杂度为$2^{\frac{N}{2}}$。之后我们维护这两部分信息，并合并解决跨子集部分。在这里我们可以用提前对A和B的子集的总和排序，并双指针技术在线性时间内找到最大跨越A、B的子集和。总的时间空间复杂度为$O(2^{\frac{N}{2}})$。