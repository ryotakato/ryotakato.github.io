---
layout: post
title: "7zaコマンドでShift-JISのzipファイルを展開する方法"
tags : [Linux]
date: 2021-04-22 09:15:44
---


Linuxでzipを解凍するときunzipを使うが、
WindowsのShift-JIS（CP932）がファイル名やディレクトリ名に入っていると化けてしまう。

なので、いつもはOオプションを指定して解凍している。

```bash
# Shift-JISをunzip
$ unzip -O SJIS hoge.zip

```

しかし、zipの中に0バイトのファイルとかが含まれていると、


```bash
# 0バイトファイル含むzip
$ unzip -O SJIS hoge.zip
  error:  invalid compressed data to inflate

```

ってな感じでエラーがでてしまって上手く解凍できない。

どうしたらいいんだろうなって探してたら、
7zaコマンドで解凍して、convmvコマンドで文字コード直せばいいというもの。

```bash
# 解凍
$ 7za x -tzip hoge.zip

# ファイル名文字コード変換(プレビュー)
# rオプションはhogeディレクトリ内を再帰的に処理
$ convmv -f shift-jis -t utf8 -r hoge/*

# ファイル名文字コード変換(実行)
$ convmv -f shift-jis -t utf8 -r -notest hoge/*
```

という手順で解凍して直すんだけど、
やってみるとうまく行かない。

どうも、7zaで解凍したときのファイル名がShift-JISじゃなさそう。



調べてみると、下記が見つかった。

[CentOSで、Windows上で圧縮された日本語を含むZIPファイルを文字化けなく解凍する方法 &#124; ミヤビッチの穴](https://hole.sugutsukaeru.jp/archives/870)



どうやら、7zaはLANG依存でファイル名を解凍してしまうので、
一時的にいじっておかないといけないらしい。

ということで最終版


```bash
# 事前確認
$ echo $LANG
ja_JP.UTF-8

# 書き換え
$ export LANG=ja_JP

# 解凍
$ 7za x -tzip hoge.zip

# 元に戻す
$ export LANG=ja_JP.UTF-8

# 事後確認
$ echo $LANG
ja_JP.UTF-8

# ファイル名文字コード変換(プレビュー)
# rオプションはhogeディレクトリ内を再帰的に処理
$ convmv -f shift-jis -t utf8 -r hoge/*

# ファイル名文字コード変換(実行)
$ convmv -f shift-jis -t utf8 -r -notest hoge/*
```


意外に手間取った。








