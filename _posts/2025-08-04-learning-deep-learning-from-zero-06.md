---
layout: post
title: "「ゼロから作る Deep Learning」で機械学習を学ぶ その06 CIFAR-10の学習の過学習を抑制"
tags : [Python, Deep Learning]
date: 2025-08-04 17:48:44
---


前回の続き。

ソースは下記
[ryotakato/de_zero](https://github.com/ryotakato/de_zero)


とりあえず過学習していたので、そこを抑制する。

MNISTの手書き文字判定ではあまり貢献していなかったDropoutを0.5の割合でいれて、
かつ、ハイパーパラメータの1つである、重みの減衰も0.01の値として実行する。



色々試した結果、



![learning_deep_learning_from_zero_06_01]({{ BASE_PATH }}/images/2025/08/04/learning_deep_learning_from_zero_06_01.png)




うーん、まあある程度過学習しないようにはなったけど、
結局のところ学習の速度が遅くなっただけのような気も。

やっぱりCIFAR-10のような画像は、CNNを使って画像として学習をさせないといけないかもしれない。


ということで、次からは本書に戻り、第7章に入っていく。

