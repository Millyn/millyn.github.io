---
title: LeetCode 【数组】类型 基础题
date: 2020-07-23 17:16:32
categories: Python
tags:
  - Golang
  - 算法
  - Hash
  - 数组
  - 交集
  - Leetcode
---

# 存在的重复元素

用 Hash 字典的思想，用数字作为字典的索引，凡是出现过的数字值 + 1，当索引的值等于 2 时，则产生重复。

```go
func containsDuplicate(nums []int) bool {
	arr := make(map[int]int)
	for i := 0; i < len(nums); i++ {
		arr[nums[i]]++
		if arr[nums[i]] == 2 {
			return true
		}
	}
	return false
}
```

# 只出现的一次数字

异或运算的应用。
任何数字与 0 异或 = 任何数字
2 与 0 异或 = 2 -- `二进制 10 + 00 = 10 = 2`

任何数字和自己异或 = 0
2 与 2 异或 = 0 -- `二进制 10 + 10 = 00 = 0`

不同的数字之间异或
1 与 2 异或 = 3 -- `二进制 01 + 10 = 11 = 3`
4 与 6 异或 = 2 -- `二进制 0100 + 0110 = 0010 = 2`

```go
func singleNumber(nums []int) int {
	ans := 0
	for i := 0; i < len(nums); i++ {
		ans ^= nums[i]
	}
    return ans
}
```

# 两数之和

双循环思路，第二重循环里只需要从 i 开始，减少操作次数。

```go
func twoSum(nums []int, target int) []int {
  result := make([]int, 2)
	for i := 0; i < len(nums); i++ {
		for j := i+1; j < len(nums); j++ {
			if (nums[i] + nums[j]) == target {
				result[0] = i
				result[1] = j
                return result
			}
		}

	}
	return result
}
```

# 移动零

双指针思路，遇到非 0 ，把右侧的数字移动到左侧。
遇到 0 对指针计数加 1 。
第一次循环，进行正向排序。
第二次循环，进行 0 的赋值。

```go
func moveZeroes(nums []int)  {
	zeroNum := 0
	for i := 0; i < len(nums); i++ {
		if nums[i] != 0 {
			nums[zeroNum] = nums[i]
			zeroNum++
		}
	}
	for i := zeroNum; i < len(nums); i++ {
		nums[i] = 0
	}
}
```

# 两个数组的交集 II

使用一个 Hash 来储存第一个数组的数字出现的数量，以 数字为 Key 出现次数为 Value
然后循环第二个数组，匹配 Hash 并判断 Value 是否为 0，
如果不为 0 ，则证明存在，则把当前数字增加到 Result 并且对 Hash 当前 Key 的 Value - 1 ，直至循环结束。

```go
func intersect(nums1 []int, nums2 []int) []int {
	result := []int{}
	hash := make(map[int]int)
	for i := 0; i < len(nums1); i++ {
		hash[nums1[i]]++
	}
	for i := 0; i < len(nums2); i++ {
		if hash[nums2[i]] != 0 {
			hash[nums2[i]]--
			result = append(result, nums2[i])
		}
	}
	return result
}
```
