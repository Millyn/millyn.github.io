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

<!-- more -->

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

# 二叉树的层序遍历

```go
var res [][]int
func levelOrder(root *TreeNode) [][]int {
    if root == nil{
        return nil
    }
    // 创建 result 的二维数组
    res = make([][]int, 0)
    dfs(root, 0)
    return res
}

func dfs(root *TreeNode, level int){
    if root == nil{
        return
    }

    // 这里是关键，按照递归的线性
    // 如果当前 层数 等于 数组的长度，则首先推进一个空的二维数组
    if level == len(res){
        res = append(res, []int{})
    }
    // 在当前层的二维数组 Push 数据
    res[level] = append(res[level], root.Val)
    // 递归先推左节点
    dfs(root.Left, level+1)
    // 递归再推右节点
    dfs(root.Right,level+1)
}
```

# 将有序数组转换为二叉搜索树

这里要讲一下，二叉搜索树的定义：

二叉搜索树（Binary Search Tree）是指一棵空树或具有如下性质的二叉树：

- 若任意节点的左子树不空，则左子树上所有节点的值均小于它的根节点的值
- 若任意节点的右子树不空，则右子树上所有节点的值均大于它的根节点的值
- 任意节点的左、右子树也分别为二叉搜索树
- 没有键值相等的节点

这一题是讲的中序遍历的过程

```go
func sortedArrayToBST(nums []int) *TreeNode {
    rand.Seed(time.Now().UnixNano())
    // 进行递归
    return helper(nums, 0, len(nums) - 1)
}

func helper(nums []int, left, right int) *TreeNode {

    // 根据定义，左节点要小于右节点，否则就不属于正确的二叉搜索树
    if left > right {
        return nil
    }

    // 选择任意一个中间位置数字作为根节点
    mid := (left + right + rand.Intn(2)) / 2
    // 创建当前 Root
    root := &TreeNode{Val: nums[mid]}
    // 向下递归衍生左叶子，此时 Left 是 0 ，right 是 中间数 - 1 ，即为左侧节点
    root.Left = helper(nums, left, mid - 1)
    // 向下递归衍生右叶子，此时 Left 是中间数 + 1，right 是 nums 的长度，即为右侧节点
    root.Right = helper(nums, mid + 1, right)
    return root
}
```
