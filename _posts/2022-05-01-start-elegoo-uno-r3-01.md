---
layout: post
title: "Elegoo Uno r3(Arduino)を始めてみた 01"
tags : [つくったもの]
date: 2022-05-01 17:54:44
---


自作キーボードを作ってから、電子工作に興味をもっているのだが、
実際に何か作ったことはなかったので、とりあえず基礎からということで、
Arduinoに手を出そうと思った。

で、一個一個パーツを調べて買うのは面倒なので、
Elegoo UNO R3 のスターターキットを買ってみた。


<div class="AmaQuick-box" style="margin-bottom: 0px;"><div class="AmaQuick-image" style="float: left; margin: 0px 12px 1px 0px;"><a href="https://www.amazon.co.jp/dp/B06XF2HZGT/?tag=tavi06-22" name="AmaQuicklink" target="_blank"><img src="https://m.media-amazon.com/images/I/61S+fEvTvML._SL200_.jpg" alt="ELEGOO Arduino用UNO R3スターターキット レベルアップ チュートリアル付 mega2560 r3 nanoと互換 [並行輸入品]" style="border: none;"/></a></div><div class="AmaQuick-info" style="margin-bottom: 10px; line-height: 120%"><div class="AmaQuick-name" style="margin-bottom: 10px; line-height: 120%"><a href="https://www.amazon.co.jp/dp/B06XF2HZGT/?tag=tavi06-22" name="AmaQuicklink" target="_blank">ELEGOO Arduino用UNO R3スターターキット レベルアップ チュートリアル付 mega2560 r3 nanoと互換 [並行輸入品]</a><div class="AmaQuick-powered-date" style="font-size: 80%; margin-top: 5px; line-height: 120%">posted with <a href="https://creazy.net/amazon_quick_affiliate" title="AmaQuick" target="_blank">AmaQuick</a></div></div><div class="AmaQuick-detail">ELEGOO</div><div class="AmaQuick-sub-info" style="float: left;"><div class="AmaQuick-link" style="margin-top: 5px"><a href="https://www.amazon.co.jp/dp/B06XF2HZGT/?tag=tavi06-22" name="AmaQuicklink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="AmaQuick-footer" style="clear: left"></div></div>


とりあえず今回はちょっと分かりづらかった最初の準備のところを備忘として書いておく。
チュートリアルが添付してあるので、だいたいその通りにやればいいが、
僕の環境はLinuxなので書いてないところもあったし。

参考したのは下記
丁寧に説明していて、わかりやすい。

[子供と一緒に始める マイコンプログラム Arduino 入門 【購入編】 | おもろ家](https://omoroya.com/arduino00/)




### PC環境

* Ubuntu 20.04 LTS



### Arduino IDEをインストール

公式サイトからダウンロードしろってあるが、Debian系だとaptでいれたほうが手っ取り早い。


```bash
# いつもの更新
$ sudo apt upgrade

# インストール
$ sudo apt install arduino arduino-core

```

で、試しに起動してみると、
最初にArduino Permission Checker というのが出て、
Ignore と Add の選択肢がある。

これは、シリアルポートへの書き込み権限を、ログインしているユーザーに持たせるかどうかっていう確認なんだけど、
ただ起動したいだけなら、Ignoreでもいい。
しかし、どうせすぐに必要になるので、
Addを選んでおくとよい。

で、ここで、Addを選ぶと確かに追加されるのだが、
ログインし直さないと、IDEを起動するたびに同じことを聞かれるので、
一度ログインし直しておいたほうがよい。

また、これをCUIでやる場合、下記を参照のこと。

[My開発メモ](http://kanako.s500.xrea.com/nukblog/show.rhtml?id=646)
[【Ubuntu】arduinoに書き込もうとしたら怒られた - ブルーの趣味Log](https://www.blue-weblog.com/entry/2017/11/23/204902)



### シリアルポートの設定

IDEをインストールしたら、
Lesson 01で、シリアルポートの設定を行う。
IDEを起動して、USBケーブルでPCとArduinoを接続したら、

IDEのメニューから、ツール → シリアルポート → /dev/ttyACM0 を選択する。
Windows向けの説明だと、選ぶシリアルポートはCOMなんとかってやつなんだけど、
Linuxだと、上記のようにdev配下にマウントされている。


Lesson 01はこれだけ。
ライブラリのインストール方法も書いてあるが、
これはここでやるやつではなく、あくまで方法を書いているだけ。



### プログラムの書き込み

Lesson 02で、書き込みのテストを行う。
IDEでプログラム（Arduinoではスケッチと呼ぶらしい）を書いたり、最初から用意されているものをロードしてきたら、
適当な名前をつけて保存する。
ここで、数字始まりはダメで、勝手にアンダースコアが足される。

また、保存すると、ファイル名と同名のディレクトリ配下にファイルが保存される。
この同名ディレクトリは必須なので、IDE以外から勝手にディレクトリを変えたりしないほうがいい。


ArduinoがUSBケーブルで接続されていることを確認したら、
マイコンボードへ書き込みのボタンを押すことで、コンパイルされ、Arduinoへ書き込まれる。

結構すぐできるので、拍子抜けする。
プログラム言語はArduino言語というCとかC++を元にした独自言語みたい。
わかりやすいからいいけど、Pythonなど直接他言語で書けないものかな。




とりあえずこんなところ。






