---
layout: post
title: "IntelliJ IDEA のインストール（）"
tags : [IDE, Linux]
date: 2020-12-17 10:17:44
---


メインで使っているUbuntuでは、開発にはVimをつかっていて、
あまりIDEは入れてなかったのだけれど、
JavaCCのjjファイルを扱うのが面倒で、IntelliJ IDEAをいれようと思った。

いつものようにaptでいれるのかなと思ってたら、

[How to install IntelliJ IDEA on Ubuntu 20.04 Linux Desktop - LinuxConfig.org](https://linuxconfig.org/how-to-install-intellij-idea-on-ubuntu-20-04-linux-desktop)
[Ubuntu20.04にIntelliJ IDEA2020.3をインストールし日本語化してみた - Qiita](https://qiita.com/maccadoo/items/a4acfdcd94c47104b02c)

をみると、aptじゃなくて、snapを使うようだ。

snap？なにそれ？新しいパッケージマネージャ？


[Snap とはこういうことか?､エンドウーザー庶民として（ubuntu系Linux） : ご年配Linux](https://vegam57.livedoor.blog/archives/6071857.html)

これをみると、
確かに新しいパッケージマネージャなんだけど、
aptとは違って、
依存関係にある各パッケージ（ライブラリ）を共有するんじゃなくて、
入れたいパッケージ毎に依存パッケージが独立している。
この仕組みには、Dockerなどで使われているコンテナ技術が使われているようだ。

なるほど、確かに依存パッケージが勝手にバージョンアップして、
挙動が変わってしまうと困るようなものではsnapのほうがいいのかもしれない。

ただ、容量やメモリは食うかもしれないね。
なんでもかんでもsnapってわけじゃなくて、
軽いものは引き続きaptな気がする。


さて前置きが長くなったが、IntelliJ IDEA
ちなみに、Ubuntu 20.04 LTS


```bash
# 事前に全てのパッケージをアップデート aptでいうupgradeと認識している。
$ sudo snap refresh
All snaps up to date.

# 無料版のCommunityをインストール
$ sudo snap install --classic intellij-idea-community
intellij-idea-community 2020.3 from jetbrains✓ installed

# 起動
$ intellij-idea-community


```

簡単！





