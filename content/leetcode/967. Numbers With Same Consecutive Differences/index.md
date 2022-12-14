---
title: "967. Numbers With Same Consecutive Differences"
categories:
- Leetcode
tags:
- Backtracking
- Breadth-First Search
date: 2022-09-03T19:10:20+08:00
draft: false
summary: 2022/09/03 leetcode日常 (Mid難度)
thumbnailImage: leetcode/avator/leetcode_icon.png
thumbnailImagePosition: right
---

{{< toc >}}

## 題目敘述

### 原文

> Return all **non-negative** integers of length `n` such that the absolute difference between every two consecutive digits is k.
>
> Note that **every** number in the answer **must not** have leading zeros. For example, 01 has one leading zero and is invalid.
>
> You may return the answer in **any order**.

### Constrains

> `2 <= n <= 9`
>
> `0 <= k <= 9`

### 解析

回傳一個非負整數陣列，其中每個數字位元長度為`n`，每個連續位元的值之間的絕對值必須為`k`，該陣列中不允許0開頭的數字，不過陣列中不強調數字的順序。

## 第一想法

我應該會採用遞迴枚舉的方式蒐集所有數字。首先指定最高位元為1~9 (因為不允許0開頭)，隨後確認下一個位元相對於目前位元的`±k`是否介於0~9之間，如果是就再往下遞迴處理。{{< hl-text red >}}但由於k可能為0{{< /hl-text >}}，所以在k為0時不能重複把+0跟-0遞迴處理，否則會發生重複值被放入。

概念與 **DFS** 雷同

```go
func numsSameConsecDiff(n int, k int) []int {

    ans := make([]int, 0)

    var dfs func(nowDigitLevel int, n int, digitNumber int, k int, nowValue int)
    dfs = func(nowDigitLevel int, n int, digitNumber int, k int, nowValue int) {
        if 0 <= digitNumber && digitNumber < 10 {
            if nowDigitLevel < n-1 {
                if k == 0 {
                    dfs(nowDigitLevel+1, n, digitNumber, k, nowValue*10+digitNumber)
                } else {
                    dfs(nowDigitLevel+1, n, digitNumber+k, k, nowValue*10+digitNumber)
                    dfs(nowDigitLevel+1, n, digitNumber-k, k, nowValue*10+digitNumber)
                }
            } else {
                ans = append(ans, nowValue*10+digitNumber)
            }
        }
    }

    for i := 1; i < 10; i++ {
        dfs(0, n, i, k, 0)
    }

    return ans
}

```

> **Success**
>
> Runtime: 0 ms, faster than 100.00% of Go online submissions for Numbers With Same Consecutive Differences.
>
> Memory Usage: 2.7 MB, less than 41.67% of Go online submissions for Numbers With Same Consecutive Differences.

{{< alert success >}}
改用閉包後這次就跑好快喔🤣
{{< /alert >}}

### 討論

解法應該這樣就好了，雖然複雜度應該是`O(2^n)`，但因為n只有到9，所以其實還算可以

## 偷看一下網路解 (70ms)

```go
func numsSameConsecDiff(n int, k int) []int {
    res := []int{}
    highest := int(math.Pow10(n))
    lowest := int(math.Pow10(n-1))

    var backtrack func(number int)
    backtrack = func(number int) {
        if lowest <= number && number < highest {
            res = append(res, number)
            return
        }
        last := number % 10
        lower := last - k 
        if lower >= 0 {
            backtrack(number * 10 + lower)
        }
        higher := last + k
        if higher <= 9 && higher != lower{
            backtrack(number * 10 + higher)
        }
    }

    for i:=1; i<=9; i++ {
        backtrack(i)
    }
    return res
}
```

概念上跟我是一樣的 (採DFS)，但是他的條件是藉由lowest與highest來確認創造出來的值(number)已經達到n位元 (挖靠這想法好狂🤣)，加上他演算法的順序做了些微調，因此遞迴函數的參數直接少了我4個😨

{{< alert info >}}
又學到一手🤣🤣🤣
{{< /alert >}}
