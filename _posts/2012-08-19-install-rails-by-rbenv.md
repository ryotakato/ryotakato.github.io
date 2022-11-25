---
layout: post
title: "Ruby on Rails インストール（rbenv使用）"
tags : [Ruby, Ruby on Rails, 環境構築]
date: 2012-08-19 15:01:23
---


今回、RubyとRailsに初挑戦。
最初なので手軽なのがいいが、プロジェクト毎にバージョンを分けたいので、
Rubyのバージョン管理ツールであるrbenvを使う。

備忘録として残しておく。
なお、いつも通りシェルスクリプトはbash。

## ■環境

* Ubuntu 12.04 LTS Desktop
* Git 1.7.9.5 インストール済み

## ■対象
* Ruby 1.9.3
* Ruby on Rails 3.2


## ■手順

### 1. Gitはインストール済みなので、飛ばす


### 2. 各種パッケージインストール

```bash
$ sudo apt-get install zlib1g-dev libssl-dev libreadline-dev libyaml-dev libxml2-dev libxslt-dev libsqlite3-dev g++
```

いくつか既にインストールされているものもあったが、問題なく終了


### 3. RVM の除去
入っていないので、飛ばす

### 4. rbenv のインストール


```bash
$ sudo apt-get install rbenv
$ echo 'eval "$(rbenv init -)"' >> ~/.bashrc
```


### 5. ruby-build のインストール


```bash
$ git clone git://github.com/sstephenson/ruby-build.git
$ cd ruby-build
$ sudo ./install.sh
```


~/にruby-buildとできるのはいやだったが、install.shの中みてみると、
どうやら/usr/local配下にコピーしているだけなので、特に影響はなさそう。


### 6. ruby のインストール


```bash
$ rbenv install 1.9.3-p194
$ rbenv rehash
$ rbenv global 1.9.3-p194
```


2012/08/19時点での最新版も参考URLにある通り、1.9.3-p194
一行目で
/usr/local/bin/rbenv-install: 行 66: rbenv-hooks: コマンドが見つかりません
と出るが、とりあえず問題ない。


```bash
$ rbenv version
```

で、1.9.3-p194 (set by /home/tavi/.rbenv/global)と出るので
問題なさそう。

しかし、

```bash
$ which ruby
```

で、既存のrubyのパス/usr/bin/rubyが表示されてしまった。

```bash
$ ruby -v
```

でも、古いのが有効になっている。


```bash
$ sudo apt-get remove ruby
```

したけど、なんだか残ってるっぽい。
ちなみに合わせて以前インストールしたALMinium（Redmine）もアンインストールした。
apache2は残ったけど、大丈夫だろう。

では、

```bash
$ sudo rm -rf /usr/bin/ruby
```

を実行（大丈夫かな？）
とりあえず、いなくなったけど、rbenvのrubyが認識されていない。


うーん、分からないので、とりあえず、bashを再起動して、
再度rubyのインストールをやり直した。

```bash
$ rbenv install 1.9.3-p194
$ rbenv rehash
$ rbenv global 1.9.3-p194
```

すると、

```bash
$ which ruby
```

で、~/.rbenv/shims/rubyと出るし、

```bash
$ ruby -v
```

でruby 1.9.3p194 (2012-04-20 revision 35410) [i686-linux]と表示される。

また、~/.rbenv/shims/には、rubyだけじゃなく、gemやrakeもはいってる。

一応できたが、既存rubyを削除したのはよかったのか？
疑問が残るがとりあえず進む。


### 7. .gemrc の作成


```bash
$ vim ~/.gemrc
```

で以下を記述

```
install: --no-ri --no-rdoc
update: --no-ri --no-rdoc
```


### 8. Ruby on Rails 3.2 のインストール

```bash
$ gem install rails
$ rbenv rehash
```

gemがちょっと長いけど、問題なし。


### 9. 作業フォルダの作成

~/develop/ruby/railsを作った。
ここにsampleとして作成する。


### 10. 動作確認用の新規アプリケーションの作成

```bash
$ cd ~/develop/ruby/rails
$ rails new sample --skip-bundle
$ cd sample
```


### 11. Gemfileの編集

```bash
$ vim Gemfile
```

下記部分を探し、コメントアウトされているのを外す

```ruby
# gem 'therubyracer', :platform => :ruby
```


### 12. 依存パッケージのインストール

```bash
$ bundle install
```

わりとすぐ終わった。

### 13. 簡単なユーザー管理機能の作成

```bash
$ rails g scaffold user name:string email:string
$ rake db:migrate
```


### 14. アプリケーションの起動

```bash
$ rails s
```


### 15. 動作確認

http://localhost:3000/usersにアクセスすると、ちゃんとページが表示される！
ふーん、確かにこりゃ簡単だ。
開発効率がいいってのもうなづける。

次は、MongoDBを使ってRailsを色々触ってみよう。

## ■参考

[Ruby on Rails 3.2 を Ubuntu にインストールする手順をかなり丁寧に説明してみました - Rails 雑感 - Ruby on Rails with OIAX](http://www.oiax.jp/rails/zakkan/rails_3_2_installation_on_ubuntu.html)


