---
title: 打家劫舍 Leetcode 198
date: 2020-07-27 10:19:35
categories: 算法
tags:
  - 算法
  - 动态规划
  - DP
  - 初级算法
---

打家劫舍是一个经典入门的 DP 算法，在开始之前，我们先了解 DP 的基本定义。

- DP 数组
- 找到子问题
  - 状态转移的方程
- 递归过程

<!-- more -->


# DP 数组

对于打家劫舍而言，DP 数组就是 `result := make([]int, len(nums))` 以 nums 的长度创建一个全部为 0 的数组。

# 找到子问题

了解打家劫舍的核心问题 **如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。** 我们按线性的思路考虑到最后一个房子偷与不偷的情况，那么就得出两个结论。

n 代表 房子的总数

偷 `n - 1` 即为倒数第二间房子，则不偷 `n`
偷 `n - 2` 即为倒数第三间房子和 `n`，则不偷 `n - 1`

## 状态转移方程

对比两种子问题得出的大小，我们解出方程 ： `f(max(dp[n-2] + nums[i], dp[n-1]))`

这个看不懂不要紧，我们可以继续往下了解。

假设输入数组为 [1,2,4,0]

我们已知 dp[0]就是 1 ，dp[1] 是 max(dp[0], dp[1])，到第三间房子的时候开始使用状态转移的方程

```
for [1,2,4,0]
i = 2 ，循环到第 4
[1 2 0 0] 当前 dp
1 --> dp[n-2]
4 --> dp[n-2] + nums[i]
2 --> dp[n-1]
Max 比较 4 和 2
[1 2 4 0] 本轮状态转移后 DP 数组

```

# 递归

这一题相对简单，我们只需要用 For 来循环整个数组即可，时间复杂度就是 O(n)

# 官方 Golang 题解

```go
func rob(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    if len(nums) == 1 {
        return nums[0]
    }
    dp := make([]int, len(nums))
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    for i := 2; i < len(nums); i++ {
        dp[i] = max(dp[i-2] + nums[i], dp[i-1])
    }
    return dp[len(nums)-1]
}

func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}

```
