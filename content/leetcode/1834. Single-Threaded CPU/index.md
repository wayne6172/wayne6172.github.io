---
title: "1834. Single-Threaded CPU"
date: 2022-12-29T13:27:27+08:00
categories:
- Leetcode
tags:
- Array
- Sorting
- Heap

draft: false
math: true
publishdate: 2022-12-30T00:00:00+08:00

summary: 2022-12-29 leetcode日常 (Mid難度)
thumbnailImage: leetcode/avator/leetcode_icon.png
---

{{< toc >}}

## 題目敘述

### 原文

> You are given `n` tasks labeled from `0` to `n - 1` represented by a 2D integer array `tasks`, where $tasks[i] = [enqueueTime_{i}, processingTime_{i}]$ means that the $i^{th}$ task will be available to process at $enqueueTime_{i}$ and will take $processingTime_{i}$ to finish processing.
>
> You have a single-threaded CPU that can process **at most one** task at a time and will act in the following way:
>
> - If the CPU is idle and there are no available tasks to process, the CPU remains idle.
> - If the CPU is idle and there are available tasks, the CPU will choose the one with the **shortest processing time**. If multiple tasks have the same shortest processing time, it will choose the task with the smallest index.
> - Once a task is started, the CPU will **process the entire task** without stopping.
> - The CPU can finish a task then start a new one instantly.
>

### 解析

這題其實挺有意思的，在我大三上的「作業系統」中，CPU Scheduling有FCFS、SJF、Priority scheduling、RR...等等，這題就是來實作SJF(Shortest Job First)

{{<alert info>}}
而這個演算法也相對其他演算法中，Process的平均等待時間比較少，可是會有機會讓行程[飢餓(starvation)](https://en.wikipedia.org/wiki/Starvation_(computer_science))，因此在作業系統的實作上還是會做一些調整，不將Shortest Job視為唯一的權重
{{</alert>}}

## Coding

這題比較麻煩的點就是在要思考Time line上，行程進入的時間、結束的時間、目前正在執行的行程、行程結束後又要從**正在等待的行程中挑最短工作時間的行程**來執行

上述的**正在等待的行程中挑最短工作時間的行程**可以使用Heap來做，只是要注意放入Heap的時間點

<!-- markdownlint-disable -->
{{< codeblock "Q1834.go" "go" >}}
package Q1834

import (
    "container/heap"
    "sort"
)

type TaskHeap [][]int

func (h TaskHeap) Len() int {
    return len(h)
}

func (h TaskHeap) Less(i, j int) bool {
    if h[i][1] != h[j][1] {
        return h[i][1] < h[j][1]
    }

    return h[i][2] < h[j][2]
}

func (h TaskHeap) Swap(i, j int) {
    h[i], h[j] = h[j], h[i]
}

func (h *TaskHeap) Push(x interface{}) {
    *h = append(*h, x.([]int))
}

func (h *TaskHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]

    *h = old[0 : n-1]
    return x
}

func getOrder(tasks [][]int) []int {
    ans := []int{}
    taskHeap := &TaskHeap{}
    heap.Init(taskHeap)

    for index := range tasks {
        tasks[index] = append(tasks[index], index)
    }

    sort.Slice(tasks, func(i, j int) bool {
        return tasks[i][0] < tasks[j][0]
    })

    nowTime := tasks[0][0]
    for i := 0; i < len(tasks); {
        for ; i < len(tasks) && tasks[i][0] <= nowTime; i++ {
            heap.Push(taskHeap, tasks[i])
        }

        if taskHeap.Len() != 0 {
            nowWork := heap.Pop(taskHeap).([]int)
            ans = append(ans, nowWork[2])
            nowTime = nowTime + nowWork[1]
        } else if i != len(tasks) {
            nowTime = tasks[i][0]
        }
    }

    for taskHeap.Len() > 0 {
        ans = append(ans, heap.Pop(taskHeap).([]int)[2])
    }

    return ans
}
{{</codeblock>}}
<!-- markdownlint-restore -->

> **Success**
>
> Runtime: 408 ms, faster than 55.56% of Go online submissions for Single-Threaded CPU.
>
> Memory Usage: 23 MB, less than 55.56% of Go online submissions for Single-Threaded CPU.

### 討論

這題除了要想何時該把行程丟入Heap的時間點，以及如果CPU遇到Idle時該如何處理比較麻煩外，這題的題目還算容易，也剛好把[昨天]({{<ref "1962. Remove Stones to Minimize the Total\index.md">}})的Heap再練習一次🤣🤣
