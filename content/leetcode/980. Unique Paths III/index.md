---
title: "980. Unique Paths III"
date: 2022-12-31T13:53:46+08:00
categories:
- Leetcode
tags:
- Array
- Backtracking
- Bit Manipulation
- Matrix

draft: false
math: true
publishdate: 2023-01-01T00:00:00+08:00

summary: 2022-12-31 leetcode日常 (Hard難度)
thumbnailImage: leetcode/avator/leetcode_icon.png
---

{{< toc >}}

## 題目敘述

### 原文

> You are given an `m x n` integer array grid where `grid[i][j]` could be:
>
> - `1` representing the starting square. There is exactly one starting square.
> - `2` representing the ending square. There is exactly one ending square.
> - `0` representing empty squares we can walk over.
> - `-1` representing obstacles that we cannot walk over.
>
> Return *the number of 4-directional walks from the starting square to the ending square, that walk over every non-obstacle square {{<hl-text red>}}exactly once.{{</hl-text>}}*

### Constraints

> - `m == grid.length`
> - `n == grid[i].length`
> - `1 <= m, n <= 20`
> - `1 <= m * n <= 20`
> - `-1 <= grid[i][j] <= 2`
> - There is exactly one starting cell and one ending cell.

### 解析

算出所有機器人從1走到2的路徑，並且有只經過一次所有的0

## Coding

由於限制只有`m * n <= 20`，因此可用DFS來解，只要是先求出必須經過的格子數量，對比已經Tracking後得到的路徑長度是否一致，就可以知道該Path是否有含全部可抵達的格子。

<!-- markdownlint-disable -->
{{< codeblock "Q980.go" "go" >}}
package Q980

func findStartAndCountNotWall(grid [][]int) (int, int, int, int, int) {
    var startX, startY int
    row := len(grid)
    col := len(grid[0])
    notWall := 0
    for x := 0; x < row; x++ {
        for y := 0; y < col; y++ {
            if grid[x][y] == 1 {
                startX, startY = x, y
            }
            if grid[x][y] != -1 {
                notWall++
            }
        }
    }

    return startX, startY, row, col, notWall
}

func uniquePathsIII(grid [][]int) int {
    startX, startY, row, col, notWalls := findStartAndCountNotWall(grid)

    var dfs func([][]int, int, int)

    length := 0
    ans := 0
    dfs = func(grid [][]int, x int, y int) {
        if x >= 0 && y >= 0 && x < row && y < col && grid[x][y] != -1 {
            length++

            if grid[x][y] == 2 && length == notWalls {
                ans++
            } else if grid[x][y] != 2 {
                grid[x][y] = -1
                dfs(grid, x+1, y)
                dfs(grid, x, y+1)
                dfs(grid, x-1, y)
                dfs(grid, x, y-1)

                grid[x][y] = 0
            }

            length--
        }
    }

    dfs(grid, startX, startY)

    return ans
}

{{< /codeblock >}}
<!-- markdownlint-restore -->

> **Success**
>
> Runtime: 6 ms, faster than 13.95% of Go online submissions.
>
> Memory Usage: 1.9 MB, less than 65.12% of Go online submissions.

### 討論

此題挺容易的，沒想到會被歸在Hard倒讓我意外...
