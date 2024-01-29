---
layout: post
title: "Godot を始めてみた＆ドキュメントの翻訳に参加してみた"
tags : [Linux, Godot]
date: 2024-01-29 20:21:44
---

先日インストールしたGodotを始めている。

現在、入門の「はじめに」と「ステップ・バイ・ステップ」が終わったところ。

[はじめに — Godot Engine (4.x)の日本語のドキュメント](https://docs.godotengine.org/ja/4.x/getting_started/introduction/index.html)
[ステップ・バイ・ステップ — Godot Engine (4.x)の日本語のドキュメント](https://docs.godotengine.org/ja/4.x/getting_started/step_by_step/index.html)


ここまでやってきて、Godotの良い点がわかってきた。

一つ目は、「シーン」というものが、Unityのそれとはかなり異なること。
Godotのシーンは単純に場面というだけではなく、
ノードの集まりとして表現されている。
そして、Unityでいうところのプレハブの意味もある。
これはオブジェクト指向的な考え方で作られたからできた芸当で、「インスタンス化」というものを上手くゲームエンジンに落とし込んでいるなーと思った。

そのため、複数ノードをまとめて「キャラクター」シーンを作り、
それを別で作った「フィールド」シーンで動かして、
「敵」シーンと戦わせる
などのことができる。
そして、各シーンは単体で動かせるから、
よくゲーム開発である、特定の動きをデバッグしたいのに、
該当のところまでメインシーンから辿っていくのに時間がかかるというのがなるべくやらなくていいようになっている。

これは大変ありがたいと思った。


二つ目は、やはり軽さ。
エンジンが軽いので、エディターもサクサク動くし、
何度も再起動して試すのが楽。

これはやる気が削がれなくて良い。


ただ、ちょっと固まってしまったので、何度かprocessを切ることがあった。
これはLinuxだからなのかよくわからないけど、まだ安定度が高くないのかもしれない。





そして、
チュートリアルを進めていく中で、
ドキュメントがところどころ翻訳されていないのに気づいたので、
翻訳プロジェクトに参加してみた。
下記のWeblateというサイトで管理しているので、
そこで原文を検索しつつ、翻訳を入れていく。

[Godot Engine/Godot Documentation — 日本語 @ Hosted Weblate](https://hosted.weblate.org/projects/godot-engine/godot-docs/ja/)


僕が翻訳されていないと思ったところは、
多くが、一度は翻訳されたが、原文が少しでも変更されたために、要編集の状態になっていたみたい。
なので、ある程度はできていたから、そこから少し日本語がおかしいところを編集して保存した。
リアルタイムで反映されるかなと思ったけど、
Weblateを実際のドキュメントに反映するのは定期的？に人がやっているみたいなので、気長に待つことにする。

[Commits · godotengine/godot-docs-l10n](https://github.com/godotengine/godot-docs-l10n/commits/4.x/)

Weblateは、提案という形にして、誰かのレビュー待ちの状態にもできるんだけど、
活動中の人が少ないとレビューもなかなかしてくれないので、
面倒なので提案はせずに保存している。
まあ、そこまで大きいプロジェクトじゃないので大丈夫でしょう。

今後もどんどんと翻訳していこうと思う。




参考
[Editor and documentation localization — Godot Engine (latest) documentation in English](https://docs.godotengine.org/en/latest/contributing/documentation/editor_and_docs_localization.html)
[Godot Game Engineの日本語ドキュメントの所在とドキュメント翻訳のお誘い #ポエム - Qiita](https://qiita.com/manzyun/items/30b10b74a73a0662b976)
[Godot の翻訳 - kidooom's Scrapbox](https://scrapbox.io/kidaaam-92022284/Godot_%E3%81%AE%E7%BF%BB%E8%A8%B3)


















