---
layout: post
title: "Node.jsインストール Macportからndenvへの乗り換え"
tags : [Node.js, JavaScript]
date: 2013-06-29 11:46:20
---



なんか最近、導入の記事ばかり書いてる気がする。
まあ、いいや。





Node.jsはサーバサイドJavaScriptとして有名だが、
インストールの方法はMacPortなどを使ってた。
しかし、この間、[@riywo](https://twitter.com/riywo)さんという方が、
ndenvというnode.jsのバージョン管理ツールを作ってくれたので、
早速試してみる。




[node.jsのバージョン管理のためにndenv & node-buildを作ったのとanyenvの宣伝 - As a Futurist...](http://blog.riywo.com/2013/06/21/152736)
[riywo/ndenv](https://github.com/riywo/ndenv)


### ■環境

* Mac OS X 10.7.5
* bash 4.2.39
* Git 1.8.0.1
* MacPortでNode.js 0.8.15をインストールしている。



### ■Macportで既存のNode.jsをアンイストール

まずはMacPortで使っているNode.jsとnpmを確認

```bash
$ port installed nodejs
The following ports are currently installed:
  nodejs @0.8.2_0+python27+ssl
  nodejs @0.8.14_0+python27+ssl
  nodejs @0.8.15_0+python27+ssl (active)

$ port installed npm
The following ports are currently installed:
  npm @1.1.68_0 (active)
```


activeになっているので、先にdeactivate
ここで、先にnpmをdeactivateしないと、エラーが出るので、注意。


```bash
$ sudo port deactivate npm
--->  Deactivating npm @1.1.68_0
--->  Cleaning npm

$ sudo port deactivate nodejs
--->  Deactivating nodejs @0.8.15_0+python27+ssl
--->  Cleaning nodejs
```


あとはアンインストール

なお、実際は古いバージョンだけアンインストールして、最新版はdeactivateで残しておいた(念のため)



```bash
$ sudo port uninstall nodejs@0.8.2_0+python27+ssl
--->  Uninstalling nodejs @0.8.2_0+python27+ssl
--->  Cleaning nodejs
```



### ■ndenvのインストール

ndenvのインストール

このブログでも書いたことのあるrbenvを参考に作られているので、手順はほとんど同じ。githubのREADME.mdを見ればだいたいわかる。


```bash
$ git clone https://github.com/riywo/ndenv ~/.ndenv
$ echo 'export PATH="$HOME/.ndenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(ndenv init -)"' >> ~/.bash_profile
$ exec $SHELL -l
$ ndenv -v
```


最後のコマンドで、ndenvのバージョンが出れば成功。


### ■node-buildのインストール

ndenv自体のインストールは完了したが、
ndenvを使って、Node.jsをインストールするには、node-buildが必要なので、インストール
ここらへんも、rbenvとruby-buildの関係と同じ。


```bash
$ git clone https://github.com/riywo/node-build.git ~/.ndenv/plugins/node-build
```


### ■使ってみる


早速Node.jsのインストール
バージョンは現時点の最新版



```bash
$ ndenv install v0.11.3
$ ndenv rehash
$ ndenv global v0.11.3
```

インストールしたら、必ずrehashが必要。
また、デフォルトバージョンにしたい場合はglobalを行う。

インストールの確認


```bash
$ node -v
v0.11.3
```

入った♪入った♪


```bash
$ ndenv install -l
```

で、インストールできるバージョンが一覧化される。


```bash
$ ndenv versions
```


で、インストール済みの一覧


これはndenvというより、rbenvに言えることなんだけど、
install -l と versionsは一緒に表示されないかな？
install -l したら、インストール済みのバージョンには印がついてる的な。
まあ、これはRVM的な発想なのだろうけど。




### ■まとめ

こういうツールがどんどんできてくるって、いいね！
[@riywo](https://twitter.com/riywo)さんに感謝！

やっぱり僕はXVM系より、xenv系のほうがシンプルで好きだな。

