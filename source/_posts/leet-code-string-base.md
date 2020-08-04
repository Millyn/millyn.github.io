---
title: LeetCode 【字符串】类型基础题回顾
date: 2020-07-23 17:16:27
categories: 算法
tags:
  - Golang
  - 算法
  - 排序
  - Leetcode
---

# 翻转字符串

使用双指针的方法进行循环，并对当前左右指针进行交换
当双指针指向同一个索引时，结束翻转。

```go
func reverseString(s []byte)  {
	left := 0
	right := len(s) - 1
	for left < right {
		s[left], s[right] = s[right], s[left]
		left++
		right--
	}
}
```

<!-- more -->

# 字符串中的第一个唯一字符

这一题不适合用 Go 做，因为在要增加一些对 byte 的认识，对算法本身来讲，不够纯粹。

```go
func firstUniqChar(s string) int {
  // 创建一个 26 位数组（英文字母）
  var arr [26]int

  // 遍历 s 对 arr 进行赋值记录每个字母的最后一次出现的所在索引
  // 以 leetcode 为例，重复的 e 第一次出现是在索引 2 ，最后一次出现是在索引 7
	for i, k := range s {
		arr[k-'a'] = i
  }
  // 赋值结果
  // [0 0 4 6 7 0 0 0 0 0 0 0 0 0 5 0 0 0 0 3 0 0 0 0 0 0]


  // 二次循环
	for i, k := range s {
    // 对 s 和 arr ，当 i 的索引（第一次出现位置，和 arr 的最后一次出现位置做比较）如果是相同，则证明是最早出现的唯一字符串（线性条件）
		if i == arr[k-'a'] {
			return i
		} else {
			arr[k-'a'] = -1
		}
	}
	return -1
}

```
