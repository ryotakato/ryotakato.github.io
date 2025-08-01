---
layout: post
title: "「ゼロから作る Deep Learning」で機械学習を学ぶ その04 Batch正規化などのテクニック"
tags : [Python, Deep Learning]
date: 2025-07-30 11:47:44
---

<style type="text/css">
<!--
table {
  border-collapse:collapse;
  border:1px solid black;
}
table th {
  border:1px solid black;
  padding-left:5px;
}

table td {
  border:1px solid black;
  padding-left:5px;
}

-->
</style>


今回は第6章「学習に関するテクニック」

ソースは下記
[ryotakato/de_zero](https://github.com/ryotakato/de_zero)



この章では、学習の精度を上げるための様々な方法を紹介している。
* 重みやバイアスの値をどの程度更新するのかの手法(SGD,Momentum,AdaGrad,Adam)
* 重みの初期値をいくつに設定するかのアイディア(Xavierの初期値, Heの初期値)
* Batch Normalizationの導入
* 過学習を抑制するための方法(Weight Decay, Dropout)
* ハイパーパラメータの最適値の与え方(学習係数, Weight Decay係数)

これらの機能を積んだクラスが、common/multi_layer_net_extend.py に実装されていたので、写経してこちらも実装してみた。
Batch Normalizationとかの仕組みは分かるが実装は自分でできなかったので、それはコピーしてきた。

でもBatch正規化がなぜ有効なのかは本書だけで分からなかったので、
調べてみたら、下記が分かりやすかった。

[バッチ正規化とは &#124; 澁谷直樹 @ キカベン](https://note.com/kikaben/n/nf0dc9446dce3)



で実装して各手法のによってどれほど精度が高くなるのかを確認してみた。



| # | 手法 | 訓練時正答率 | テスト時正答率 |
| :--- | :--- | ---- | ---- |
| 1 | 5章+SGD | 92.09% | 92.24% |
| 2 | 5章+AdaGrad | 95.80% | 95.54% |
| 3 | 6章+AdaGrad | 95.51% | 95.28% |
| 4 | 6章+AdaGrad+Heの初期値 | 96.89% | 96.13% |
| 5 | 6章+AdaGrad+Xavierの初期値 | 96.28% | 95.73% |
| 6 | 6章+AdaGrad+Heの初期値+Dropout | 93.78% | 93.87% |
| 7 | 6章+AdaGrad+Heの初期値+Batch正規化 | 98.26% | 96.92% |
| 8 | 6章+AdaGrad+Heの初期値+Dropout+Batch正規化 | 95.01% | 94.29% |
| 9 | 6章+AdaGrad+Heの初期値+Dropout+隠れ層(100)を1つ追加 | 94.95% | 94.69% |
| 10 | 6章+AdaGrad+Heの初期値+Batch正規化+隠れ層(100)を1つ追加 | 99.86% | 98.14% |
| 11 | 6章+AdaGrad+Heの初期値+Batch正規化+隠れ層(100)を1つ追加+ハイパーパラメータ最適化 | 99.86% | 98.05% |


(2025/08/01 追記 ここから)

上記表で6以降がHeの初期値って書いてあるが、実際はXavierの初期値だった。
大きな数値の違いはなさそうなので、6以降の比較が間違っているわけではないと思う。

(追記ここまで)

1,2を比較すると、やはりAdaGradのほうがいい精度出ている。
3は今回の章のMultiLayerNetExtendクラスを使った感じ。だけど、隠れ層などのノード数は同じ。
4,5は重みの初期値の確認のため。微妙な差だがHeの初期値のほうが良い結果を出ているのは、activation関数をReluでやっているため。
これをSigmoidに変えたら、Xavierのほうが良いだろうと思う。

6,7,8はDropoutとBatch Normalizationの威力を確認。
隠れ層が少ないからか、Dropoutはそこまで効果を発揮せず(むしろ精度が悪くなった)、
Batch Normalizationはかなり効果がある。

9,10は、100ノードの隠れ層を1つ追加して、
入力層、隠れ層(100)、隠れ層(50)、出力層
でやってみた。
やはりここでもBatch Normalizationは強力。

最後の11は、本章の最後に記載されていた理想的なハイパーパラメータを適用してみたが、
そう大きな変化は見られなかった。





とりあえず今回はここまで。
Deep Learningにおいて、Batch Normalizationの威力が高いことが確認できたし、
他にもチューニングできる部分というのを知ることができた。

次は7章のCNNに入っていく。

