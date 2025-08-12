---
layout: post
title: "「ゼロから作る Deep Learning」で機械学習を学ぶ その07 CNNを実装して、MNISTとCIFAR-10を学習させてみる"
tags : [Python, Deep Learning]
date: 2025-08-12 16:27:44
---


前回の続き。

ソースは下記
[ryotakato/de_zero](https://github.com/ryotakato/de_zero)


前回までで全結合のニューラルネットワークのCIFAR-10の正答率が60%にもいかなかったので、
一旦7章のCNNを実装してからやってみた。

その結果、下記となった。
どちらもepochは20

MNIST (iteration: 12000回、学習にかかった時間: 917秒、テスト正答率: 99.8%)

![learning_deep_learning_from_zero_07_01]({{ BASE_PATH }}/images/2025/08/12/learning_deep_learning_from_zero_07_01.png)






CIFAR-10 (iteration: 10000回、学習にかかった時間: 1211秒、テスト正答率: 61.2%)

![learning_deep_learning_from_zero_07_02]({{ BASE_PATH }}/images/2025/08/12/learning_deep_learning_from_zero_07_02.png)




なんとか60%いったー。
でも、まだ畳み込み層やプーリング層のパラメータを調整すればもっと改善しそうだな。
また、AlexNETなどを参考に層を重ねてみるとどうかな。

次はそこら辺を試してみる。



