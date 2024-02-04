---
layout: post
title: "Godotで最初の2Dゲーム作成"
tags : [Godot]
date: 2024-02-04 16:23:44
---

Godotをはじめてから、ようやく下記の最初の2Dゲームの終わりまでいった。

[最初の2Dゲーム — Godot Engine (4.x)の日本語のドキュメント](https://docs.godotengine.org/ja/4.x/getting_started/first_2d_game/index.html)


少しづつやってたし、理解しながらやっていたので歩みは遅いけど、
とりあえずできたのがこんな感じ。


![godot_sample2d_01]({{ BASE_PATH }}/images/2024/02/04/godot_sample2d_01.gif)


BGMはつけなかったし、
あと、チュートリアル通りだと、僕のキーボードでは全然避けることができなかったので、
自分の速度を5倍にしたり、敵の速度と生成ペースを半分にしてようやく遊べるゲームになった。

チュートリアルやった感じ、結構UIやキャラクターが作りやすそうな感じ。
ただ、まだどのオブジェクトにどういう機能があるかが全然わからないので、
自分の作りたいものが作れるには調べないといけないところが多そう。

そして、Linux版だからなのかよく分からないけど、
頻繁にGodotエディターがフリーズする。

これは困ったので調べていたら、Single Window Modeにすると良いと意見があったので、
メニューの「Editor」->「Editor Settings」の中で「Single Window Mode」を検索してONにすると、
それ以降不自然なフリーズはなくなったので、Linuxならこっちが良いと思った。




