---
title: 合并两个有序链表 LeetCode 21
date: 2020-08-04 15:06:30
categories: 算法
tags:
  - Golang
  - 算法
  - 链表
  - 递归
  - Leetcode
---
根据 LeetCode 该题较好的解答，我整理了对应的注释，看起来会更舒服。

原作信息

作者：ivan1
链接：https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/yao-xue-hui-xian-she-zhi-shao-bing-jie-dian-by-iva/

<!-- more -->

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    // 创建一个 链表节点，原作者是指哨兵节点
    prehead := &ListNode{}
    result := prehead

    // 循环 l1 和 l2 这 2 个链表
    for l1 != nil && l2 != nil {
        // 如果 l1 的链表小于 l2 的链表值，那么把 l1 推入链表，并把 l1 指向 l1 链表的下一个值
        if l1.Val < l2.Val {
            prehead.Next = l1
            l1 = l1.Next
        }else{
          // 如果 if 判断失败，反向同理
            prehead.Next = l2
            l2 = l2.Next
        }
        // 这里我到是看了半天，实际上就是把链表进行链接，否则在循环内永远修改的都是第一个链表值
        prehead = prehead.Next
    }
    // 边界处理，如果循环完了 l1 还有值
    if l1 != nil {
        prehead.Next = l1
    }
    // 边界处理，如果循环完了 l2 还有值
    if l2 != nil {
        prehead.Next = l2
    }
    // 返回 result
    return result.Next
}
```
