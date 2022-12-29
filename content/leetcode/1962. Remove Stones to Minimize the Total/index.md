---
title: "1962. Remove Stones to Minimize the Total"
date: 2022-12-28T13:36:31+08:00
categories:
- Leetcode
tags:
- Heap

draft: false

math: true

publishdate: 2022-12-29T00:00:00+08:00
summary: 2022/12/28 leetcode日常 (Mid難度)
thumbnailImage: leetcode/avator/leetcode_icon.png
thumbnailImagePosition: right
---

{{< toc >}}

## 題目敘述

### 原文

> You are given a **0-indexed** integer array piles, where `piles[i]` represents the number of stones in the $i^{th}$ pile, and an integer k. You should apply the following operation **exactly** `k` times:
>
> - Choose any `piles[i]` and **remove** `floor(piles[i] / 2)` stones from it.
>
> **Notice** that you can apply the operation on the **same** pile more than once.
>
> Return *the **minimum** possible total number of stones remaining after applying the `k` operations*.
>
> `floor(x)` is the **greatest** integer that is **smaller** than or **equal** to `x` (i.e., rounds `x` down).

### 解析

這題主要是要把piles中所有石頭數量最小化，而每次都只能從一個pile拿走一半的量，並且該操作只能做`k`次

## Coding

使用貪心法每次找piles陣列中有最多石頭的，並拿走一半，執行`k`次，由於要找**piles陣列中最大的值**並且**該陣列的值會浮動**，因此使用資料結構`Heap`來處理

```go
package Q1962

import (
    "container/heap"
)

type IntHeap []int

func (h IntHeap) Len() int {
    return len(h)
}

func (h IntHeap) Less(i, j int) bool {
    return h[i] > h[j]
}

func (h IntHeap) Swap(i, j int) {
    h[i], h[j] = h[j], h[i]
}

func (h *IntHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]

    *h = old[0 : n-1]
    return x
}

func minStoneSum(piles []int, k int) int {
    h := IntHeap(piles)
    heap.Init(&h)

    for ; k > 0; k-- {
        h[0] -= h[0] / 2
        heap.Fix(&h, 0)
    }

    ans := 0
    for _, value := range piles {
        ans += value
    }

    return ans
}

```

> **Success**
>
> Runtime: 370 ms, faster than 100% of Go online submissions for Remove Stones to Minimize the Total.
>
> Memory Usage: 8.7 MB, less than 100% of Go online submissions for CRemove Stones to Minimize the Total.

### 討論

(上述程式已經經過了許多參考並進行最佳化了)

此次題目是我第一次使用Golang的Heap，真的是初見殺

過程中一直把`heap.Push(h, x)`寫成`h.push(x)`，導致Heap的lib根本沒去做操作，破壞掉整個Heap的結構，這點除錯還看了很久🤣🤣

可能之後在使用Golang自己的`contain`時要格外注意用法就是了...

此外，也忘記Heap是可以用Fix來更新值，不必一直重新Pop、Push導致效能下降 (兩次的)

題目不難(一開始就想到解法，是對語言的不熟悉😥)
