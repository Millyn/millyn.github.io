---
title: LeetCode 【排序和搜索】类型基础题回顾
date: 2020-07-23 17:16:27
categories: 算法
tags:
  - Golang
  - 算法
  - 排序
  - 二分法
  - Leetcode
---

```go
func firstBadVersion(n int) int {
    left := 1;
    right := n;
    for left < right  {
      // 运用二分法，获取当前的中间位置来进行检查
        mid := (left + right) / 2
        if (isBadVersion(mid)) {
            // 如果是 true ，则证明中间到右侧都不可能是
            right = mid
        } else {
            // 如果是 false ，中间（包含中间）到左侧都不可能是
            left = mid + 1
        }
    }
    return left
}
```