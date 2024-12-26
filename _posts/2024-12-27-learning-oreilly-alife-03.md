---
layout: post
title: "「作って動かすALife」で人工生命を学ぶ その03 BOIDSモデル"
tags : [Python, 人工生命]
date: 2024-12-27 12:19:44
---


人工生命の続き。

今回はBOIDSモデル。
BOIDSはよく知っているが、実際に実装したことはなかったので、やってみると意外と単純な仕組みで動いているなと思った。
群れを表すので、虫とかでもいいのだが、僕には鳥にしか見えない。

ソースは下記。今回からGithubにあげてみた。
[ryotakato/alife_oreilly_src](https://github.com/ryotakato/alife_oreilly_src)


で、単なるBOIDSだけだと面白くないので、
せっかくならと、書籍にはないけど、
植物食の鳥（オレンジ色）と、それを捕食する鳥（灰色）の2種類の群れを実装してみた。
実際に捕食されて数が減るわけではないけど、群れが逃げる様子を見たかった。
さらに、植物の餌を示す、緑の球体がランダムで出現するようにして、
オレンジ色の鳥がそれに群がる感じ。
特に2枚目なんかは緑の植物に近づきたいのだけど、灰色が集まって来たから逃げている様子が撮れているなと思う。

![learning_oreilly_alife_03_01]({{ BASE_PATH }}/images/2024/12/27/learning_oreilly_alife_03_01.png)



![learning_oreilly_alife_03_02]({{ BASE_PATH }}/images/2024/12/27/learning_oreilly_alife_03_02.png)





GIF動画も。
実装した空間は3次元なので、分かりづらいが、まあ動きが想像はできると思う。

![learning_oreilly_alife_03_03]({{ BASE_PATH }}/images/2024/12/27/learning_oreilly_alife_03_03.gif)


次は5章。



