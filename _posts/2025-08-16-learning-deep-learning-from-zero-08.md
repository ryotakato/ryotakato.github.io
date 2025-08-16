---
layout: post
title: "「ゼロから作る Deep Learning」で機械学習を学ぶ その08 CNNを深くする"
tags : [Python, Deep Learning]
date: 2025-08-16 16:46:44
---


前回の続き。

ソースは下記
[ryotakato/de_zero](https://github.com/ryotakato/de_zero)


前回まででCIFAR-10の正答率がCNNによって60%に達したので、
今度は8章のCNNの層を深くすることでどれくらい変わるのかをチェック。


その結果、下記となった。
どちらもepochは20

MNIST (iteration: 12000回、学習にかかった時間: 4000秒、テスト正答率: 99.6%)

![learning_deep_learning_from_zero_08_01]({{ BASE_PATH }}/images/2025/08/16/learning_deep_learning_from_zero_08_01.png)






CIFAR-10 (iteration: 10000回、学習にかかった時間: 4649秒、テスト正答率: 73.5%)

![learning_deep_learning_from_zero_08_02]({{ BASE_PATH }}/images/2025/08/16/learning_deep_learning_from_zero_08_02.png)



層を深くしたことによって、学習にかかる時間が4倍ぐらいに伸びた。
そのおかげで、CIFAR-10は73％までいったわけだけど、
MNISTのほうはほとんど変わらないというか0.2％下がった。
本書でも言及されていたが、MNISTのような単純なやつは層を深くしてもあまり変わらないみたいだ。

うーん、さらに精度をあげるためにはどうしたらいいかな。
とりあえず各フィルターでDeap Learningが画像をどういう認識をしているかを見てみるのがいいかな。








