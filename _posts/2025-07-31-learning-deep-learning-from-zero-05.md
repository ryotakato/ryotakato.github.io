---
layout: post
title: "「ゼロから作る Deep Learning」で機械学習を学ぶ その05 CIFAR-10,100を学習させてみた"
tags : [Python, Deep Learning]
date: 2025-07-31 14:20:44
---



今回は第7章ではなく、第6章までの機能を使って、CIFAR-10を学習させてみる。


ソースは下記
[ryotakato/de_zero](https://github.com/ryotakato/de_zero)


CNNに入る前に、全結合のネットワークを使って、
手書き文字以外のものも学習させてみようかと思った。

最初はImageNetとかOpenImageなどが必要かとおもっていたんだけど、
どうも少なくても300GBぐらい容量が必要で、
流石に試すのにそこまで大きいのはいやなので、もっと軽いのを探すと、CIFAR-10というのが見つかった。
32x32で3チャンネルの画像が60000枚(50000枚訓練データ、10000枚テストデータ)
ダウンロードしても200MBいかないぐらいなのでお手軽に試せる。



でこれを読み込むプログラム組んで、
さらにそれを、
* 隠れ層は100,100,50の3層構造
* Heの重みの初期値
* Batch Normalizationあり
* Dropoutはなし
* 学習は、AdaGrad

のネットワークに通す。

最初10000回だけだと、そこまで時間がかかるわけではないので、
10倍の10万回にあげてみたら、かかった時間は478秒(8分弱)

訓練データは5万枚で、バッチサイズが100なので1 epochは500イテレーション
結果的に200 epoch

損失関数の変化はこんな感じで、いい感じに下がっているかなと思っていたのだが、

![learning_deep_learning_from_zero_05_01]({{ BASE_PATH }}/images/2025/07/31/learning_deep_learning_from_zero_05_01.png)


正答率の変化はこんな感じ。
訓練データに対して、テストデータの正答率が低い。
というか、学習していくに従い、少し下がっている。

![learning_deep_learning_from_zero_05_02]({{ BASE_PATH }}/images/2025/07/31/learning_deep_learning_from_zero_05_02.png)


最終的な結果としては、訓練の正答率94.74% に対して、テスト時正答率は46.85 %と、かなり低くなっている。
これは訓練データに過学習しすぎたか？
epochを進んでいくと正答率落ちているのはそれが原因かも。
というか、これなら10万回もやらなくてよかったことになる。



ちなみに、CIFAR-100という、100個に分類した画像のほうで学習させてみると、

![learning_deep_learning_from_zero_05_03]({{ BASE_PATH }}/images/2025/07/31/learning_deep_learning_from_zero_05_03.png)


![learning_deep_learning_from_zero_05_04]({{ BASE_PATH }}/images/2025/07/31/learning_deep_learning_from_zero_05_04.png)


同様の結果となり、
訓練正答率 59.79 %
テスト正答率 18.47 %
という結果に。

そりゃ、100個の中から当てずっぽうで正解するよりはずっと高いけど、
20%切っているとそこまで意味がない。



とりあえずテスト正答率が上がらないほうから解決してみようかな。
過学習のせいかもしれず、
それならDropoutやハイパーパラメータの変更でいけるかもしれない。

もっと精度をあげる話は、一旦それが終わってから。
もしかしたら全結合のネットワークではこれが限界でCNNを使わないといけないのかもしれないし。


参考

[CIFAR-10 and CIFAR-100 datasets](https://www.cs.toronto.edu/~kriz/cifar.html)
[カラー画像のデータセットを探し求めて &#124; Webシステム開発／教育ソリューションのタイムインターメディア](https://www.timedia.co.jp/tech/wo/)
[いまさらCIFAR-10をダウンロードして画像として保存する #Python - Qiita](https://qiita.com/typecprint/items/014c11329329a666dbd7)
[[Python]CIFAR-10, CIFAR-100のデータを読み込む方法 #Python - Qiita](https://qiita.com/supersaiakujin/items/5e9d2b2850e256f99982)





