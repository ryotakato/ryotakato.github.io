---
layout: post
title: "Ubuntuで、Brotherプリンターを使ってスキャンできるようにするまで"
tags : [Linux]
date: 2021-03-29 10:07:44
---



Ubuntuでスキャンする必要があったので、やったことまとめ。
Brotherがドライバー配布しているので、
Linuxならどのディストリビューションでもできると思う。



### 環境

* Ubuntu 20.04 LTS
* Brother DCP-J952N



### 手順

まずここからドライバーをダウンロードしておく

[ソフトウェアダウンロード &#124; DCP-J952N-B/W/ECO &#124; 日本 &#124; ブラザー](https://support.brother.co.jp/j/b/downloadlist.aspx?c=jp&lang=ja&prod=dcpj952n&os=128)

スキャナードライバー（Linux）ってところ。64bitの2ファイルと設定ファイル。


次にnmapで自身のプリンターの内部IPアドレスを調べておく。
nmapがなければaptでいれておく。

```bash
$ sudo nmap -sn 192.168.1.0/24

（中略）
Nmap scan report for 192.168.1.3
Host is up (0.015s latency).
MAC Address: 00:00:00:00:00:00 (Hon Hai Precision Ind.)

```
僕のだとこんな感じででる。



では、ドライバーインストール
Brotherのサイトでは、dpkgでいれるコマンドが書いてあるけど、
aptで管理しておきたいのでapt版


```bash
# スキャナードライバーのインストール
$ sudo apt install ./brscan4-0.4.9-1.amd64.deb

# スキャンキーツールのインストール
$ sudo apt install ./brscan-skey-0.3.1-1.amd64.deb


# 設定ツールのインストール
$ sudo apt install ./brother-udev-rule-type1-1.0.2-0.all.deb
```

いれおわったら、設定。
さっき調べたIPアドレスと、プリンターの型番、
あと名前を設定する。
名前はなんでも良さそうなので、わかりやすく、型番と同じで。


```bash
# 設定
$ sudo brsaneconfig4 -a name=DCP-J952N model=DCP-J952N ip=192.168.1.3
```


実際にスキャンするアプリは、
Xsaneを使いたいのでインストール。



```bash
# xsaneをインストール
$ sudo apt install xsane
```


あとはxsaneを起動すれば勝手に探してくれる。



### 参考

[Ubuntu Linux で BROTHER 複合機のスキャナを使う – TURNIP 2](https://femoghalvfems.info/archives/24297)


