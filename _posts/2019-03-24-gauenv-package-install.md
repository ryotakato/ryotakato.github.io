---
layout: post
title: "Gaucheのバージョン管理ツール gauenv でライブラリのインストール"
tags : [Gauche, つくったもの]
date: 2019-03-24 12:22:44
---

備忘も兼ねて。  

gauenvでGaucheを使っていて、ライブラリのインストールが必要になったので、やり方を整理してみた。  

Gaucheは0.9.3ぐらいからgauche-packageというのが実装されており、  
tar.gzになった状態のライブラリをそのままインストールできるようになった。  
しかし、最近の傾向として、皆ライブラリを作ったら、Githubに公開ということをするので、  
あまりtar.gzで公開していない。  
勿論、releaseをちゃんと作っていればいいが、皆そうだとは限らない。  
なので、git cloneからのやり方で。  
サンプルはsqlite3ライブラリ。  


```bash
$ git clone https://github.com/mhayashi1120/Gauche-dbd-sqlite3.git
$ cd Gauche-dbd-sqlite3
$ ./DIST gen
$ cd ../
$ tar -zcvf Gauche-dbd-sqlite3.tar.gz Gauche-dbd-sqlite3
$ gauche-package install Gauche-dbd-sqlite3.tar.gz 

# gauenvを使っている場合はこれも必要
$ gauenv rehash
```


一度git cloneしてから、DISTしておき、自身でtar.gzにまとめて、gauche-packageでインストールする。  
非常に回りくどい気がするが、とりあえずこれで用は足りる


