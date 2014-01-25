---
layout: post
title: "BundlerでRailsを始める手順＆スクリプト"
tags : [Ruby,Ruby on Rails]
date: 2013-02-10 01:43:02
---

久々にRubyを更新し、
久々にRailsをやってみようと思ったので、
アプリを作成するときの手順をまとめようと思う。

gemの管理にはBundlerを使っており、
各アプリケーションが競合しないようにRailsさえもローカルインストールする方法を選んだ。

また、僕はMongoDBが好きなので、MongoDBの場合の手順もちょいちょい加えていく。




## 環境

* Mac OS X 10.7.5
* bash 4.2.29
* git 1.7.11.2


## 手順概要

* rbenvでRubyインストール
* Bundlerインストール  ※ ここまで、毎回不要
* Rails初期設定Gemfile作成
* Railsインストール
* アプリケーション新規作成
* 初期設定Gem削除
* アプリケーションGemインストール
* アプリケーション起動テスト


ここを<s>パクった</s>参考にしたww
[Rails開発環境の構築（rbenvでRuby導入からBundler、Rails導入まで）](http://qiita.com/items/a60886152a4c99ce1017)


## rbenvでRubyインストール

もうすでに何回か書いているので、省略
下記を参照
[Mac に git で rbenv と ruby をインストール](/viewEntry?id=20121110155041)
[rbenv で ruby の最新を使う](/viewEntry?id=20130208072630)


## Bundlerインストール


```bash
# Bundlerインストール
$ rbenv exec gem install bundler
$ rbenv rehash

# 確認
$ rbenv exec gem list
```


## Rails初期設定Gemfile作成

ここから、Railsアプリケーションを作成していく


```bash
# ディレクトリ作成
$ mkdir /path/to/app

# cd
$ cd /path/to/app

# Gemfile作成 Railsのバージョンは、その都度変える
$ cat << EOS > Gemfile
source "http://rubygems.org"
gem "rails", "3.2.11" 
EOS

```

## Railsインストール

```bash
$ bundle install --path vendor/bundle
```

completeと出ればOK
Gemfile.lockができている

## アプリケーション新規作成

ここでは、アプリケーション名をexampleとしている

```bash
# 通常の場合
$ bundle exec rails new example --skip-bundle

# MongoDBを使う場合
$ bundle exec rails new example --skip-bundlea --skip-active-record
```

--skip-bundleしないと、一気にbundle installまで走ってしまう。
あとでbundle installするので、ここでは飛ばす


## 初期設定Gem削除

アプリを作ったら、このgemは不要

```bash
$ rm Gemfile
$ rm Gemfile.lock
$ rm -rf .bundle
$ rm -rf vendor/bundle
```

そして、現在example配下にあるrailsアプリを丸ごと一階層上にもってくる
この作業は別にしなくてよいが、ディレクトリが深くなるのがいやなので、やっておく。
つまり、/path/to/app/exampleがRailsアプリだったのが、/path/to/appとなるようにする。
最初からこのつもりなら、/path/to/appディレクトリも、exampleという名前にしておけばよい。

```bash
$ mv -f example/* .
$ mv example/.gitignore .
$ rm -rf example
```


## アプリケーションGemインストール

ここで、アプリのbundle installを行うが、
MongoDBを使う場合、MongoDB用のGemも必要なので、以下をGemfileに追記

```ruby
gem 'mongoid'
gem 'bson_ext'
```

bundle install

```bash
# Gemインストール
$ bundle install --path vendor/bundle

# .gitignoreに追記
$ echo '/vendor/bundle' >> .gitignore
```


## アプリケーション起動テスト

```bash
$ bundle exec rails server
```

これで、localhost:3000を開き、Railsのindex.htmlが出れば成功


## 最後に

あれ？これ、スクリプトにすればよくね？
ってことで、スクリプトにしたのがここにありまーす。（３分クッキング方式）
はじめてGist使ったけど、簡単でいいね！

{% gist 4745892 %}


