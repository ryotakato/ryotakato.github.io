---
layout: post
title: "asdfでGO言語のインストールと、様々なライブラリの調査"
tags : [Linux, 環境構築]
date: 2024-01-24 07:19:44
---

以前GO言語(以下、golang)を使って、じゃんけんライフゲームみたいなの作ってたんだけど、
環境が変わって、ソースしか残ってなかった。
改めてまたやろうかなと思ったので、asdfでgolangいれてみた


```bash
$ asdf plugin update --all
$ asdf plugin add golang
$ asdf install golang 1.21.6
$ asdf global golang 1.21.6
$ asdf reshim
$ go version
go version go1.21.6 linux/amd64
```


よしよし。

で、実際に動かそうとしたら、OpenCVを使っていたのを忘れてて、エラー。
うーん、今更OpenCV使う？

どうしようと色々ぐるぐる考えながらネットサーフィンしてたら、
色々エンジンやライブラリ周りがわかってきたのでメモ。

* ebiten: ゲームエンジン　超シンプル
* go-sdl2: SDLというマルチメディアライブラリをgolangで使うためのもの。SDLはOpenGLやDirectXと同じ位置づけだが3Dグラフィックはもっていないらしい。
* glfw: OpenGLを扱うためのライブラリ。
* go-box2d-lite: 物理エンジンのbox2d-liteをgolangから使うためのもの。box2d-liteはかなり機能は少ないらしいので超シンプル。
* fyne: golangのGUIライブラリ。同様のライブラリにほshinyってのもあるらしい。
* gocv: golangからOpenCVを使うためのもの。
* godot-go: godotというゲームエンジンをgolangから使うためのもの。

参考
[GoでクロスプラットフォームGUI(2022)](https://zenn.dev/nobonobo/articles/6cc4c510988e82)


で、ここまできて、Godotというゲームエンジンが最近流行ってきていることを知った。
上記godot-goで、golangからも使えるが、Pythonちっくなスクリプト言語らしいので、使ってみてもいいなと思った。
Unityみたいにエディターが重いってこともなさそうだし。
（僕の中でUnityは料金体系変更のやらかしもそうだけど、エディターの重さもなんとなく敬遠する理由になっている）


そうなると、今回golangいれた意味は・・・？
まあ、いいか。


