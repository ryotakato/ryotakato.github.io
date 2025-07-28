---
layout: post
title: "Mac M4 にてSteamをプレイするには"
tags : [Mac, 環境構築]
date: 2025-07-29 09:05:44
---


Steamで様々なゲームをプレイするには、Windows一択という現状は知っている。
だけど、以前LinuxでもProtonなどを使い、動かすことができたので、
Macでもできないかを調べて実際に動かしてみた。




今回参考にしたのは下記。


[MacでWindows版Steamを起動してMouthwashingをプレイする #wine - Qiita](https://qiita.com/myo_m/items/86b36abed5766ac7c41f)
[MacでWindows版Steamを使用する方法(無料のWineskinServer) #wine - Qiita](https://qiita.com/yaju/items/51af9434bb859453f82b)
[Whisky is no longer maintained, now what? (A Guide) : r/macgaming](https://www.reddit.com/r/macgaming/comments/1k98zht/whisky_is_no_longer_maintained_now_what_a_guide/)


軽く試してみたい程度だし、できないならそれでいい感じなので、
CrossoverやParallel Desktopを使うみたいにお金かかる方法はパス。
で、無料だとWhisky（開発終了）や、その後継であるKegworksがよいというので、
Kegworksを試した。

結論からいうと、Steamは問題なく動き、
とりあえず試しでInscryptionをやってみると問題なく遊べた。



### 環境情報

まずは環境情報。
MacBookAir の、2025 M4 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		15.5
BuildVersion:		24F74
```


### Kegworksインストール

インストールはbrewで。
なお、どこかでRosettaを入れないといけないってのを見かけた気がするが、
すでに入れているので省略。

```bash
$ brew upgrade
$ brew install --cask --no-quarantine Kegworks-App/kegworks/kegworks
```




### ラッパーアプリを作成

基本的には一個目のリンクの通りやっていく。

2025年07月時点だと、installed engineにインストールできるのは、
WS12WineCX24.0.7_2
だった。

Wrapper Versionは、
Wineskin-3.1.7_2



### Steamとfakejapaneseインストール

作ったラッパーアプリを起動して、
Winetricks
というメニューから、
steamとfakejapaneseを探してインストール

fakejapaneseは日本語化しないならいらないけど、
一応僕はsteamで信長の野望とか太閤立志伝とか持っているから、一応入れておく。




### 設定

ラッパーアプリを起動した段階だと実行するファイルがまだ設定されていないので、
Browseから "C:\Program Files (x86)\Steam\steam.exe" を探して指定する。




### Steam実行


Test Runを押せば起動した。
少し時間かかる。
Steamアカウントでログインして、
持っているなにかのゲームをダウンロード
今回はInscryptionにした。途中までやってクリアしてない。

ゲームのダウンロード時、94%で途中で止まってしまったので、おかしいなって思ってSteam自体を再起動したら、ダウンロード完了していた。


で、Inscryption起動したら、普通に遊べる。
DXVKもいらない。



ただし、ずっとやっているとMacが少し熱くなってくる。
無理させているのかな。

とりあえず後はウィズダフネを動かせるのかを試してみたのだけど、
ダウンロードで止まっちゃって

[Steam Kegworks—Having trouble downloading a particular game (corrupt download?) : r/macgaming](https://www.reddit.com/r/macgaming/comments/1kx4gd4/comment/n1jk31c/)

の通りにsh作ってダウンロード。

しかし、起動しない。

il2cppというDLLがおかしいよってエラーメッセージ出たから、
Unityのこういうビルドで作ったゲームは無理かな。

[il2cpp error : r/wine_gaming](https://www.reddit.com/r/wine_gaming/comments/1dhkj4n/il2cpp_error/)

とか見たけど、ダメっぽい。
まあ、ウィズダフネはスマホでできているから別にいいや。











