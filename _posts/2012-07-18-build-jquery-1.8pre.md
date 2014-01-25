---
layout: post
title: "jQueryをビルドしてみた ver 1.8pre"
tags : [jQuery, JavaScript]
date: 2012-07-18 23:56:56
---


## ■環境など

* Mac Book Air ( Mac OS Lion )
* コマンドは全てbash
* Git インストール済み（Git 1.7.5.4）
* MacPortsインストール済み（MacPorts 2.1.1）


## ■動機
jQueryのソースが読みたいと思った。
だけど、普通にリリースバージョン読んでも面白くないので、どうせなら最新版を読んでみようと思った。
本日時点での最新版は1.8pre


## ■手順

まず大きく分けて、以下

1. githubからクローン
2. node.jsとnpmのインストール
3. jQueryのビルド

ふう、手間がかかるなー。。。



### 1. githubからクローン

gitはインストールしてあったので、

```bash
$ git clone git://github.com/jquery/jquery.git
```


をするだけなのだが、今回はAndroidでも読みたいために、Dropboxの専用ディレクトリ内部に作成。



### 2. node.jsとnpmのインストール

jQueryをビルドするためには、node.jsとnpmが必要だと知った。
どこからインストールするかと思ったけど、MacPortsがすでに存在したので、MacPortsを使う。
以下を参考にしながらインストールした。
[MacPortsを使用してOSX Lionにnode.jsをいれてみる | tkd55 blog](http://www.tkd55.net/blog/?p=92)

まずは、node.jsのインストール

```bash
$ sudo port selfupdate
$ sudo port install nodejs
$ node -v
```


上記コマンドで、v0.8.2と出たので、正常にインストールされていることが分かる。


次に、npmのインストール
これは上記サイトを参考に、wgetを使う
そのため、一度別のディレクトリに移動して、install.shを持ってくる。


```bash
$ sudo port install wget
$ wget http://npmjs.org/install.sh
$ sudo sh install.sh
$ npm -v
```


`1.1.43` と表示されたので、npmのインストールも完了。



### 3. jQueryのビルド

ここから先は、jQueryのgithubにあるREADMEを参考にしたので、

```bash
$ cd jquery && npm install
$ grunt
```


でイケる筈なのだが、駄目だった。
まず、npmでおこられる。
どうやら、僕の環境だとnpm installにはsudo権限が必要なようだ。何故かは分からない。

次に、gruntなんていうコマンドは存在しないと言われる。
おそらく、npm installでは、jqueryディレクトリ内に`node_modules`ができるので、
グローバルなコマンドとしてはインストールされていないみたいだ。
なので、以下のコマンドにより、gruntをグローバルインストールした。

```bash
$ sudo npm install -g grunt
```


よって、最終的には、ビルドに必要なコマンドは以下だった。

```bash
$ cd jquery
$ sudo npm install
$ sudo npm install -g grunt
$ grunt
```



これで、distディレクトリが作成され、その中に、jquery.jsとjquery.min.jsが！
これでビルドが完了した。次からはgruntするだけでいい。




