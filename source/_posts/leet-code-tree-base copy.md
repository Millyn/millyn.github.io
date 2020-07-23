---
title: LeetCode 【树】类型基础题回顾
date: 2020-07-23 18:56:37
categories: 算法
tags:
  - Golang
  - 算法
  - 排序
  - Leetcode
---

# 二叉树的最大深度

经典递归过程，通过递归获取左右树比较后的最大值，最终 + 1 返回（因为树自身还存在 1 级）

```go
func maxDepth(root *TreeNode) int {
   if root == nil {
        // 触底条件
        return 0
    } else {
        leftDepth := maxDepth(root.Left)
        rightDepth := maxDepth(root.Right)
        return int(math.Max(float64(leftDepth), float64(rightDepth)) + 1)
    }
}
```

# 验证二叉搜索树

```go
func isValidBST(root *TreeNode) bool {
	// 边界检查，如果 root 不存在，返回 true
	if root == nil {
		return true
	}
	// 进入递归
	return isBST(root, math.MinInt64, math.MaxInt64)
}

func isBST(root *TreeNode, min, max int) bool {
	// 这里要考虑，我们是按层遍历，所以还会有一层 root 是否存在的判断，如果上一层没有 return ，证明上一层是符合要求且包含子树。
	// 这里很关键
	if root == nil {
		return true
	}

	// 按照规则进行比较
	if min >= root.Val || max <= root.Val {
		return false
	}
	// 如果算法进行到这一步，证明当前的节点既满足比较规则，又有下一级，所以我们要继续向下获取结果
	// 当向下获取结果时，如果下一级 root 是 nil ，则直接返回 true，因为我们已经通过所有的校验，树的遍历也结束。
	return isBST(root.Left, min, root.Val) && isBST(root.Right, root.Val, max)
}
```

# 对称二叉树

```go

func isSymmetric(root *TreeNode) bool {
    if root == nil {
        return true
		}
    return isTree(root.Left , root.Right)

}

func isTree(left *TreeNode, right *TreeNode) bool {
		// 如果左节点和右节点都不存在，则返回 True ，证明要么是 [1] ，要么孩子都检查完了没有遇到异常
    if left == nil && right == nil {
        return true
		}

		// 如果左节点或右节点有一个非 nil ，证明不对称
    if left == nil || right == nil {
        return false
    }

		// 如果左节点的 Value 不等于右节点的 Value
    if left.Val != right.Val {
        return false
    }

		// 进入递归，传入左节点的左叶子和右节点的右叶子，以左节点的右叶子和右节点的左叶子进行递归比较
    return isTree(left.Left, right.Right) && isTree(left.Right, right.Left)
}
```
