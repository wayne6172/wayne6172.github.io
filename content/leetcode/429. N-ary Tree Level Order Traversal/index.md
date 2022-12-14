---
title: "429. N-Ary Tree Level Order Traversal"
date: 2022-09-05T20:12:27+08:00
categories:
- Leetcode
tags:
- Tree
- Breadth-First Search
- Leetcode Complete

draft: false
summary: 2022/09/05 leetcode日常 (Mid難度)
thumbnailImage: leetcode/avator/leetcode_icon.png
thumbnailImagePosition: right

---

{{< toc >}}

## 題目敘述

### 原文

> Given an n-ary tree, return the level order traversal of its nodes' values.
>
> Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).

### Constraints

> - The height of the n-ary tree is less than or equal to `1000`
> - The total number of nodes is between `[0, 10^4]`

### 解析

給一棵樹，回傳以陣列表示的levelOrder，注意此樹不一定為binary tree.

### 第一想法

用BFS來處理，但是需要標記這個節點目前的level，以便能放入指定的level中。

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

type levelNode struct {
    Node
    Level int
}

func levelOrder(root *Node) [][]int {

    queue := make([]levelNode, 0)
    ans := make([][]int, 0)

    if root == nil {
        return ans
    }
    
    
    queue = append(queue, levelNode{
        Level: 0,
        Node:  *root,
    })


    for len(queue) != 0 {
        nowNode := queue[0]

        if len(ans) == nowNode.Level {
            ans = append(ans, []int{nowNode.Val})
        } else {
            ans[nowNode.Level] = append(ans[nowNode.Level], nowNode.Val)
        }

        for i := 0; i < len(nowNode.Children); i++ {
            queue = append(queue, levelNode{
                Node:  *nowNode.Children[i],
                Level: nowNode.Level + 1,
            })
        }

        queue = queue[1:]
    }
    return ans
}
```

> **Success**
>
> Runtime: 4 ms, faster than 67.21% of Go online submissions for N-ary Tree Level Order Traversal.
>
> Memory Usage: 6.1 MB, less than 7.38% of Go online submissions for N-ary Tree Level Order Traversal.

{{< alert warning >}}
Memory好像不太妙...
{{< /alert >}}

### 第一想法：討論

果然用BFS，但是記憶體看來是因為直接配置了新的結構 (levelNode)導致...

## 偷看一下網路解 (4.5MB)

```go
func levelOrder(root *Node) [][]int {
    if root == nil {
        return [][]int{}
    }

    ans:=[][]int{}
    q:=[]*Node{root}
    for len(q)>0 {
        n:=len(q)
        level:=[]int{}
        for i:=0;i<n;i++{
            root, q = q[0],q[1:]
            level = append(level,root.Val)
            for _,c:=range root.Children {
                q = append(q,c)
            }
        }
        ans = append(ans,level)
    }
    return ans
}
```

### 網路解：討論

原來如此...最外層的for迴圈來處理queue裡面所有的節點，並且保證這個queue中的節點必定在同一層。不是像我一樣一邊pop一邊放入，而是藉由先知道這個level的節點有幾個 (然後設為變數`n`)，再把queue中所有節點的所有子節點放入queue中(一邊放入一邊pop，但迴圈是用`n`控制)，這樣就不用額外用變數去紀錄目前節點的level了。

喔對，原來queue的pop可以用`nowNode, q = q[0],q[1:]`這樣寫法。(Golang特性)
