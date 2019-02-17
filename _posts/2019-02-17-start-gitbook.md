---
layout: post
title: "Gitbookをやってみた"
tags : [Gitbook, Node.js]
date: 2019-02-17 17:32:44
---


最近設計書を書くのにいいのがないかなと探していたら、Gitbookというのを見つけたので、  
とりあえず試してみる  



### 環境

* Ubuntu 18.04 LTS
* ndenv 


### Node.jsの最新バージョンインストール

node.jsを使うのだけど、  
僕はndenvを入れてバージョン管理しているので、  
最新のnode.jsを入れる  


```bash
$ ndenv install v11.10.0
・・・コマンド結果は省略・・・
$ ndenv global
$ ndenv rehash
```

node.jsの他のバージョン使っている場合は別にglobalにしなくても良い。localでそのディレクトリだけで使うようにすれば良い。  


### Gitbookインストール


```bash
$ npm install -g gitbook-cli
$ ndenv rehash
```


-gにしているのは、gitbookコマンドを使うから  
ndenv使っているなら、npmでインストールした後はrehash必須。  
数分もかからずインストール完了  


バージョン確認

```bash
$ gitbook --version
```


### 基本的な使い方

ディレクトリを作って初期化するまで  

```bash
$ mkdir sample-gitbook
$ cd sample-gitbook

$ gitbook init
```



テスト用のドキュメントを書いて、htmlを生成する。  
なお、buildでhtmlを生成するのは、htmlをgit pagesなどにあげるときだけで良い。  
この後のserveはbuildしなくてもできるみたい。  

```bash
$ vim test.md
$ gitbook build

```


書いたドキュメントを見る  
下記コマンドでサーバ起動して、その後 [localhost:4000](http://localhost:4000)を開けばみれる。  

```bash
$ gitbook serve

```





とりあえずこれで試しはできた。  
後はテンプレート機能とかないかな。  
ちょっと色々やってみる予定。  










