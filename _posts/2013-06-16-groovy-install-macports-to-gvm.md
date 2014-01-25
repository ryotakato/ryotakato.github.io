---
layout: post
title: "Groovyインストール MacportからGVMへの乗り換え"
tags : [Groovy]
date: 2013-06-16 22:41:18
---


いままで、GroovyはMacPortで管理してきたが、
GVMという、RubyでいうRVMみたいなツールがあるということで、
そちらに乗り換えてみることにした。

GVMはいわゆるバージョン管理ツールで、
Groovyはもちろんのこと、
GradleやGrails、Griffonなど、周辺のフレームワークなどもバージョン管理できる点が特徴。
なによりインストールが簡単。


それでは早速。


### ■環境

* Mac OS X 10.7.5
* bash 4.2.39
* curlインストール済み
* MacPortでGroovyとGradleをインストールしている


### ■Macportで既存のGroovyやGradleをアンインストール

まずはMacPortで使っているGroovyやGradleを確認

```bash
$ port installed groovy
The following ports are currently installed:
  groovy @1.8.5_0
  groovy @1.8.6_0(active)

$ port installed gralde
The following ports are currently installed:
  gradle @0.9.2_0
  gradle @1.2_0
  gradle @1.3_0(active)
```

activeになっているので、先にdeactive

```bash
$ sudo port deactivate groovy
--->  Deactivating groovy @1.8.6_0
--->  Cleaning groovy

$ sudo port deactivate gradle
--->  Deactivating gradle @1.3_0
--->  Cleaning gradle
```


あとはアンインストール
なお、実際は古いバージョンだけアンインストールして、最新版はdeactiveで残しておいた(念のため)


```bash
$ sudo port uninstall gradle@0.9.2_0
--->  Uninstalling gradle @0.9.2_0
--->  Cleaning gradle
```



### ■GVMのインストール

GVMのインストール curlがあれば一発

```bash
$ curl -s get.gvmtool.net | bash
```


なお、ここで`JAVA_HOME`が設定されていなければ、エラーになるので注意
正常なら、GVMのアスキーアートが出てインストール完了となる。


### ■設定

インストール時に、`.bash_rc`と`.bash_profile`に、GVMのsource行が追記されているので、
実は特に設定は必要ないが、
僕の環境では`.bash_profile`から`.bash_rc`を読み込んでいるので、
`.bash_profile`にはsourceは必要ない。

よって、`.bash_profile`から該当行を削除

再度bashログインすればgvmコマンドが使える。


### ■使ってみる


早速Groovyのインストール
バージョンは現時点の最新版


```bash
$ gvm install groovy
```

途中でこのバージョンをデフォルトにしていいか聞かれるので、yesを選択

インストールの確認

```bash
$ groovy --version
Groovy Version: 2.1.5 JVM: 1.7.0_09 Vendor: Oracle Corporation OS: Mac OS X
```

簡単だね！
では次に、以前のバージョンをインストールしてみる。

```bash
$ gvm install groovy 1.8.6
```

今回はデフォルトにしない。


```bash
$ gvm list groovy
```

で、インストール済みのバージョンや、デフォルトのバージョンを確認できる。
ここには載せないが、たぶん、見たら一発。

ついでにGradleも最新版いれておこう

```bash
$ gvm install gradle
```



### ■まとめ

超絶便利
皆もGVMにしたらいいよ。
Groovy楽しいなー！


### ■参考

[GVM: Groovy enVironment Manager - uehaj's blog](http://uehaj.hatenablog.com/entry/2012/10/25/062921)
[GVM by gvmtoolをMacに導入 - MofuMofuFarm](http://d.hatena.ne.jp/jin_kojima/20130101/1357000274)
[Groovyをbrewからgvmに乗り換えるメモ - 日々常々](http://d.hatena.ne.jp/irof/20130129/p1)



