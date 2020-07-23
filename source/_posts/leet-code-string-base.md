---
title: LeetCode 【字符串】类型基础题回顾
date: 2020-07-23 17:16:27
categories: Python
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