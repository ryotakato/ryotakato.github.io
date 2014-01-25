---
layout: post
title: "Mac に git で rbenv と ruby をインストール"
tags : [Ruby]
date: 2012-11-10 15:50:41
---


以前、Ubuntuには[rbenvをインストール](../../../08/19/install-rails-by-rbenv/)したが、
Macには入れてなかったので、インストール
MacPortsやHomebrewは使わず、Githubから落としてきてインストール
難しいものではないが、Ubuntuにインストールしたときははまったので、備忘録として。
以下の環境で一応すでにRuby 1.8.7が入っていたが、それでも問題なくインストールできた。

## ■環境

* Mac OS X 10.7.5
* bash 4.2.29
* git 1.7.11.2



## ■手順

### 1. rbenvのダウンロードと初期設定


```bash
$ git clone git://github.com/sstephenson/rbenv.git .rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

なお、このあと、bashを再起動する必要あり。


### 2. ruby-buildのダウンロードとインストール

rbenvだけだと、rubyのビルド自体は自分でしなければならないので、ruby-buildをいれて自動化する。

```bash
$ git clone git://github.com/sstephenson/ruby-build.git
$ cd ~/ruby-build/
$ sudo ./install.sh
```


### 3. Rubyのインストール


```bash
$ rbenv install 1.9.3-p194
$ rbenv rehash
$ rbenv global 1.9.3-p194
```

ここでは、前と同じ1.9.3-p194をインストールしたが、別に最新でかまわない。

### 4. 確認


```bash
$ ruby -v
```

ちゃんと"1.9.3-p194"がインストールできていることを確認できた。
readlineなどのインストールが必要かと思ったが、前に入れてたかもしれず、
エラーなどは起きなかった。完了。



