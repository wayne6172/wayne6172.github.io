---
title: "2022-12-28 Record"
date: 2022-12-28T14:34:16+08:00
categories:
- "Blog Update"
tags:
- MathJax
- Katex

draft: false

math: true

publishdate: 2022-12-29T00:00:00+08:00
summary: 將MathJax改成Katex，並修正反斜線的轉譯問題、將Markdownlint的MD033關掉
# thumbnailImage: //example.com/image.jpg
# thumbnailImagePosition: right
---

## 前情提要

在[昨日]({{<ref "2022-12-27_Record">}})增加了MathJax後發現了一些轉譯上的問題，今天來把它徹底解決掉

## 解決

感謝[此篇文章](https://vincentthh35.csie.org/p/%E5%A6%82%E4%BD%95%E5%9C%A8-hugo-%E5%85%A7%E5%B5%8C-latex-%E6%95%B8%E5%AD%B8%E5%BC%8F/)的大大告知Hugo有反斜線的轉譯問題，以及他也介紹了Katex這個工具，因此我決定直接跳到Katex(超快決定😂)

{{<alert info>}}
會採用Katex除了因為它在Github的星星數量遠超MathJax之外，它轉譯的時間確實也比MathJax快上很多
{{</alert>}}

因此本次除了更換成Katex後，也新增了一個Shortcode：math來處理反斜線在Hugo上會被吃掉的問題

原始碼：

```latex
$$
\begin{pmatrix}
1 & 2 & 3\\
a & b & c
\end{pmatrix}
$$
```

Render結果：
{{<rawHtml>}}
$$
\begin{pmatrix}
1 & 2 & 3\\
a & b & c
\end{pmatrix}
$$
{{</rawHtml>}}

因為這次加上了`math`標籤來避免掉 Markdown renderer吃掉反斜線

## 總結

由於Latex的數學式很常會遇到反斜線作為函數或特殊用途，但除此之外一般的式子不需要在沒有反斜線時還需要特別加上`math`的標籤，直接用`$`括起來即可
