---
title: "{{ replace .ContentBaseName "_" " " | title }}"
date: {{ .Date }}
categories:
- Leetcode
tags:
- tag1
- tag2

draft: false
math: true
publishdate: {{ time.Format "2006-01-02T15:04:00-07:00" (time ((.Date | time.AsTime).AddDate 0 0 1 | time.Format "2006-01-02") "Asia/Taipei") }}

summary: {{ time.Format "2006-01-02" .Date }} leetcode日常 (XXX難度)
thumbnailImage: leetcode/avator/leetcode_icon.png
---

{{< toc >}}

## 題目敘述

### 原文

>

### Constraints

>

### 解析

## Coding

<!-- markdownlint-disable -->
{{< codeblock "XXX.go" "go" >}}

{{< /codeblock >}}
<!-- markdownlint-restore -->

> **Success**
>
> Runtime: XXX ms, faster than XXX% of Go online submissions.
>
> Memory Usage: YYY MB, less than YYY% of Go online submissions.

### 討論
