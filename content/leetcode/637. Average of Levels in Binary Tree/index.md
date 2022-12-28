---
title: "637. Average of Levels in Binary Tree"
date: 2022-09-02T12:50:58+08:00
categories:
- Leetcode
tags:
- Tree
- Depth-First Search
- Breadth-First Search
- Binary Tree
- Leetcode Complete

draft: false
summary: 2022/09/02 leetcode日常 (Easy難度)
thumbnailImage: leetcode/avator/leetcode_icon.png
thumbnailImagePosition: right
---

{{< toc >}}

## 題目敘述

### 原文

> Given the `root` of a binary tree, return **the average value of the nodes on each level in the form of an array**. Answers within `10^-5` of the actual answer will be accepted.

### Constraints

>
> - The number of nodes in the tree is in the range `[1, 10^4]`.
> - `-2^31 <= Node.val <= 2^31 - 1`

### 解析

給一棵樹，回傳每個level中所有節點中的值的平均值(以陣列表示)。

### 第一想法

用DFS來計算每一層的值總和以及節點數量來取得平均值，但要注意的是值域會到正負`2^31`，因此要小心overflow。這邊就用Golang的`int64`來處理應該就好了。

```go
type Solution struct {
    LevelSum   []int64
    LevelNodes []int
}

func NewSolution() Solution {
    return Solution{
        LevelSum:   make([]int64, 10000),
        LevelNodes: make([]int, 10000),
    }
}

func (solution *Solution) dfs(root *TreeNode, level int) {
    if root != nil {
        solution.LevelSum[level] += int64(root.Val)
        solution.LevelNodes[level] += 1

        solution.dfs(root.Left, level+1)
        solution.dfs(root.Right, level+1)
    }
}

func averageOfLevels(root *TreeNode) []float64 {
    solution := NewSolution()
    solution.dfs(root, 0)

    maxLevel := 0
    for i := 0; i < 10000; i++ {
        if solution.LevelNodes[i] == 0 {
            maxLevel = i
            break
        }
    }

    ans := make([]float64, maxLevel)
    for i := 0; i < maxLevel; i++ {
        ans[i] = float64(solution.LevelSum[i]) / float64(solution.LevelNodes[i])
    }

    return ans
}
```

> **Success**
>
> Runtime: 19 ms, faster than 23.36% of Go online submissions for Average of Levels in Binary Tree.
>
> Memory Usage: 7 MB, less than 14.95% of Go online submissions for Average of Levels in Binary Tree.

{{< alert warning >}}
Runtime 百分比僅有 23.36%，Memory狀態也不是很理想
{{< /alert >}}

### 第一想法：討論

後來其實有想過用BFS來解...只是Golang的queue不知道該怎麼實作，應該說要怎麼做才能不必直接定義出很大的記憶體空間，而是改用動態配置的方式來New。

其實應該是可以直接土法煉鋼...但有點太懶了🤣，所以才導致記憶體跟Runtime都那麼爛吧

## 偷看一下網路解 (70ms)

```go
func averageOfLevels(root *TreeNode) []float64 {
    
    result := make([]float64, 0)  
    
    if root == nil {
        return result
    }
    
    sum := make([]int, 0)
    total := make([]int, 0)
    
    var traverse func(n *TreeNode, level int) 
    traverse = func(n *TreeNode, level int) {
        
        if n == nil {
            return
        }
        
        if level > len(total) {
            sum = append(sum, n.Val)
            
            total = append(total, 1)
        } else {
            
            sum[level - 1] += n.Val
            total[level - 1]++
        }
        
        traverse(n.Left, level + 1)
        traverse(n.Right, level + 1)
    }
    
    traverse(root, 1)
    
    for i := 0; i < len(sum); i++ {
        result = append(result, float64(sum[i])/float64(total[i]))
    }
    
    return result
}
```

### 網路解：討論

阿對齁，完全忘記可以用 **閉包(Closure)** 的概念來寫，然後也忘記make時的buffer放0後，用append去動態配置，完成一個簡易的queue😨，這點我確實該打屁股😥

## 結論

總之，至少這次多少又喚醒了Golang的一些記憶，果然不常用的語言久了都會生疏，寶刀未老什麼的都是浮雲罷了🤣

至少這次記得了

1. make中的size為0時的陣列，可以用append來動態配置，來達到簡易的queue實作
2. 可以使用閉包(Closure)來實作函數內的遞迴函數，並直接用區域變數來模擬閉包中的全域變數
