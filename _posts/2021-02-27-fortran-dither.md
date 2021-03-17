---
layout: post
title: "fortranでdither法を使って2値化をする"
category: "fortran"
excerpt: ""
math: true
---

# {{ page.title }}

大津の2値化ではあるしきい値を持って全体を0,1に分けていたが、dither法では0,1をうまく使って中間色を表現しようとする手法である。

今回はdither matrixのパターンとして、Bayer法を使う。

```
+----+----+----+----+
| 0  | 8  | 2  | 10 |
+----+----+----+----+
| 12 | 4  | 14 | 6  |
+----+----+----+----+
| 3  | 11 | 1  | 9  |
+----+----+----+----+
| 15 | 7  | 13 | 5  |
+----+----+----+----+
```

画像を4\*4の小領域に分け、各領域とmatrixを比べ、matrixより大きいければ1、小さければ0とする。

fortranで実装すると以下のようになる。

<script src="https://gist.github.com/Omochice/18ee43cc97cc9bb9754d2c404bca3f48.js"></script>

列優先なのでmtxの宣言が転置して状態で宣言していることに留意する必要がある。

また、画像と直接比較するため、mtxを`0~maximum`に拡張している。

imgから切り出した4\*4の小領域とmtxを比較して、小領域を0,1で置き換え、rstにそれぞれおいていくイメージになっている。
（直接rstに書き込んでいくようにすれば高速化の可能性がある。）

実際に適用すると次のようになる。
![dither](/gh-pages/images/dither.jpg)