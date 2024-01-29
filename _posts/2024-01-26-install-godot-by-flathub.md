---
layout: post
title: "flathubでGodot Engineのインストール"
tags : [Linux, 環境構築, Godot]
date: 2024-01-26 06:41:44
---

先日書いたGodot Engineをインストールしようとしたときのメモ。

最初snapで入れたんだけど、上手く起動しなかったので、公式が勧めているflathubでのインストールに変えた。
flathubは今回まで知らなかったんだけど、またインストール経路が一つ増えるのか。。。
仕方ない。

まずはflathubのインストール。


```bash
$ sudo apt update
$ sudo apt install flatpak
$ sudo apt install gnome-software-plugin-flatpak
$ flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

ここまでやったら再起動。

関係ないけど、起動時のGrubの選択で、Windowsが選べないようになってた。
なんでだ。まあたまにしか使わないから、あとで見ておこう。


で、起動したら、いよいよGodotのインストール。


```bash
$ flatpak install flathub org.godotengine.Godot
```

もっと軽いと思ってたから、結構サイズあるんだよね。全部で1.5GBぐらいかな。
もちろん、Unityの11GBとかより全然軽いけど。


では、Godotを起動してみよう。

```bash
$ flatpak run org.godotengine.Godot
```

最初はコマンドからだけど、今後はプログラムのアイコンからでもいいよね。


![install_godot_01]({{ BASE_PATH }}/images/2024/01/26/install_godot_01.png)





参考
[Ubuntu用Flathubのセットアップ &#124; Flathub](https://flathub.org/ja/setup/Ubuntu)
[Godot &#124; Flathub](https://flathub.org/ja/apps/org.godotengine.Godot)



