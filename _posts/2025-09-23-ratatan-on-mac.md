---
layout: post
title: "MacにてRatatanのアーリーアクセス版ができた！"
tags : [Mac, 環境構築, ゲーム]
date: 2025-09-24 00:02:44
---

2年以上前に投稿したRatatanの記事。
[パタポンの精神的続編ラタタンのKickStarterが異常](/2023/08/03/ratatan-at-kickstartar)


ようやくアーリーアクセス版が届いたのだが、
あの時と違ってPCがMacしかないので、できないかなーって思ってた。

しかし、MacにkegworksでSteamインストールしたらできたので、記しておく。
Macだと動かないゲームも多い中、これはすごいなーと思う。

なお、この情報はこの記事書いた時点なので、後でできなくなる可能性があります。
また、基本Ratatan公式ではWindows対応しか謳っていないので、自己責任でお願いします。



まず、

[Mac M4 にてSteamをプレイするには](/2025/07/29/steam-on-mac)

に従って、MacにKegWorksをいれます。



そして、次にRatatanのアーリーアクセス版をゲットするため、
ブラウザ版のSteamに入って、
下記URLから、コード入力画面にいきます。

https://store.steampowered.com/account/registerkey


開発元からBackerKitで届いたコード入力をすれば、引き換え完了になります。


MacでSteamを開き、自身のライブラリにRatatanがあるか確認します。
そこで、インストールボタンを押してダウンロードを進めるのだけど、
おそらく少しまったらすぐにエラーで止まってしまうと思う。

なので、一度Steamは閉じ


[Steam Kegworks—Having trouble downloading a particular game (corrupt download?) : r/macgaming](https://www.reddit.com/r/macgaming/comments/1kx4gd4/comment/n1jk31c/)

の通りにsh作って、ダウンロード。

実行権限与えて、./Downloads ディレクトリにでも配置しておく。
で、RatatanのIDは、 2949320 なので、

これを引数に上記shを実行。

すると、5分ぐらいでダウンロードとインストール完了。



再度Steamを開き、Ratatanを起動すれば、普通に始まる！






Switch版発売までおあずけかと思っていたから、すごい嬉しい。




今学校のレポートに集中しないといけないのに、めっちゃやりたくなってこれは困ったぞー。










