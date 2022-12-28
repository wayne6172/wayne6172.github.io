---
title: "2022-12-27 Record"
date: 2022-12-27T12:19:28+08:00
categories:
- Blog
tags:
- "Blog Update"

math: true

draft: false
publishdate: "2022-12-28T00:00:00+08:00"

summary: MathJax測試，版本調整為3.2.2
---

## 已知問題

1. 所有矩陣類型的都會有誤，換行的標籤得用```\\\```而非```\\```，待釐清
2. ```$```的關鍵字無效，儘管已經在```tranquilpeak\layouts\partials\script.html```中的```inlineMath```加了也沒用
3. 嵌入於句中得用```\\( \\)```而非```\( \)```
4. 獨立於行中得用```\\[ \\]```而非```\[ \]```

## Test

需加入```math: true```標籤

\\[ \cos (2\theta) = \cos^2 \theta - \sin^2 \theta \\]

$$ \lim\limits_{x \to \infty} \exp(-x) = 0 $$

$$ f(n) = n^5 + 4n^2 + 2 |_{n=17} $$

$$ \frac{n!}{k!(n-k)!} = \binom{n}{k} $$

\\[ \int_0^\infty \mathrm{e}^{-x} \mathrm{d}x \\]

這是一個式子 \\(P\left(A=2\middle|\frac{A^2}{B}>4\right)\\)

## 目前有錯的 (已於12/28修正)

{{<math>}}
$$
\begin{pmatrix}
1 & 2 & 3\\
a & b & c
\end{pmatrix}
$$
{{<\math>}}
