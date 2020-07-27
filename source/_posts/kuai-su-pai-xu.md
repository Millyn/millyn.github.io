---
title: Python 快速排序
date: 2019-10-24 15:34:15
categories: Python
tags:
- Python
- 算法
- 排序
---

根据 《算法导论》 里对快速排序的原则描述
编写其代码，并解释代码；

``` python
a = [5,2,4,6,1,3]

for i in range(1, len(a)):
    key = a[i]  # 从第二位开始，拿起 2 （排序位）
    j = i - 1   # key 值取的是 被排序位的索引 - 1，也就是拿到在 key 值前面一位的索引值
    while j >= 0 and key < a[j]:
        # 如果 j 这个索引位大于等于 0，排序位的值小于 j （前一位）的值
        a[j+1] = a[j]
        # 把 j 索引位的值挪动到后一位
        j -= 1
        # j - 1 也就是说，j 会成为再靠前的一位，或者是 - 1 ，如果是 -1 则证明没有前一位，则结束 while
    a[j+1] = key
    # 当结束 while 后 j 索引位如果不是 -1 就是当前的 key 值 小于 a 数组里某一个值的索引位
    # 所以 j + 1 = key 值，让 key 值去到排序后的位置

print(a)
```