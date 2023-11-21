---
layout: post
title: "asdfでNode.jsのインストールと、簡易HTTPサーバの用意"
tags : [Linux, 環境構築]
date: 2023-11-22 04:34:44
---


Node.jsをインストールしたかったというより、
簡易HTTPサーバが欲しくて、
Pythonでもできるけど、昔に使ったnode-staticが使いやすかったなと思い、
asdfでNode.jsをインストールした。


```bash
$ asdf plugin update --all
$ asdf install nodejs latest
$ asdf global nodejs latest
$ asdf reshim
$ node -v
v21.2.0
```


よしよし。


で、ここで調べて見ると、
今はnpx serveですぐにサーバを建てられるみたい。

そりゃいいやと思い、叩いてみる。

```bash
$ npx serve
Need to install the following packages:
serve@14.2.1
Ok to proceed? (y)
```

ありゃ？
どうやら、LTSバージョンじゃないとデフォルトではインストールされていないみたい。
かといって、最新版をLTSに下げるのもなーってことで、serveをいれることにする。
ただし、ここでyとすると、ローカルに入りそう。
だけど、HTTPサーバはよく使うと思うから、全体に入れる。
ということで、nにしておいて、


```bash
$ npm install -g serve
$ asdf reshim 
```

でインストール。


いよいよということで、serve起動。

```bash
$ npx serve
(node:721491) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)

   ┌─────────────────────────────────────────┐
   │                                         │
   │   Serving!                              │
   │                                         │
   │   - Local:    http://localhost:3000     │
   │   - Network:  http://192.168.1.6:3000   │
   │                                         │
   │   Copied local address to clipboard!    │
   │                                         │
   └─────────────────────────────────────────┘

 HTTP  11/21/2023 9:37:12 AM ::1 GET /
 HTTP  11/21/2023 9:37:12 AM ::1 Returned 200 in 110 ms
 HTTP  11/21/2023 9:37:12 AM ::1 GET /favicon.ico
 HTTP  11/21/2023 9:37:12 AM ::1 Returned 404 in 8 ms
```


良かった良かった。

ちなみに、久々にHTMLを生で書いていたけど、
もう時代はHTML5ではなくなっていた。
かといってHTML6がでたわけではなく、
色々内紛があったらしく、
HTML Living Standard
というのが最新みたいだ。

詳しい経緯は下記参照。

[どうしてHTML5が廃止されたのか | フューチャー技術ブログ](https://future-architect.github.io/articles/20210621a/)





