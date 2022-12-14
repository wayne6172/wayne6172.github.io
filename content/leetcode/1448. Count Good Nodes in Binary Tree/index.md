---
title: "1448. Count Good Nodes in Binary Tree"
categories:
- Leetcode
tags:
- Tree
- Depth-First Search
- Breadth-First Search
- Binary Tree
- Leetcode Complete
date: 2022-09-01T14:47:35+08:00
draft: false
summary: 2022/09/01 leetcode日常 (Mid難度)
thumbnailImage: leetcode/avator/leetcode_icon.png
thumbnailImagePosition: right
---

{{< toc >}}

## 題目敘述

### 原文

> Given a binary tree ```root```, a node X in the tree is named **good** if in the path from root to X there are no nodes with a value greater than X.
>
> Return the number of **good** nodes in the binary tree.
>
> **Constraints:**
>
> - The number of nodes in the binary tree is in the range [1, 10^5].
> - Each node's value is between [-10^4, 10^4].

### 解析

這題簡單來說就是從```root```到*X*節點的路徑中所有的節點，是否存在比*X*節點中的值還要更大的值。若不存在，則*X*節點即為 good node。題目則問該tree中有多少個good node？

## 第一想法

這題我是打算用 **Depth-First Search (DFS)** 來解，在進行DFS時帶著目前走訪過的路徑中最大的值(這裡假設叫```max```)，若新的點比該值還大，則此點為good node，並且更新```max```值，最後統計有幾個good node即可。

```go
func dfs(node *TreeNode, maxValue int) int {
    if node == nil {
        return 0
    }

    if maxValue > node.Val {
        return dfs(node.Left, maxValue) + dfs(node.Right, maxValue)
    } else {
        return dfs(node.Left, node.Val) + dfs(node.Right, node.Val) + 1
    }
}

func goodNodes(root *TreeNode) int {
    return dfs(root, root.Val)
}
```

> **Success**
>
> Runtime: 195 ms, faster than 5.99% of Go online submissions for Count Good Nodes in Binary Tree.
>
> Memory Usage: 10.5 MB, less than 80.84% of Go online submissions for Count Good Nodes in Binary Tree.

{{< alert warning >}}
Runtime 百分比僅有 5.99%
{{< /alert >}}

### 討論

水啦：難得Mid的難度能夠立刻寫出來，而且說不上太難。

但奇怪的是，速度非常糟糕，可能是因為使用了遞迴的關係，導致Golang的效能降低。

## 偷看一下網路解 (70ms)

```go
func goodNodes(root *TreeNode) int {
    return dfs(root, -11111)
}

func dfs (root *TreeNode, max int) int {
    if root == nil {
        return 0
    }
    res := dfs(root.Left, mx(max, root.Val)) + dfs(root.Right, mx(max, root.Val))
    if max <= root.Val {
        res ++
    }
    return res
}

func mx(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

痾...這跟我的解法其實蠻像的...而且我自己Copy Paste後我反而跑了195ms...真奇怪...

算了，確定想法跟別人一樣後，懶得思考了🤣
