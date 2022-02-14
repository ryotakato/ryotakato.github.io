---
layout: post
title: "anyenvとnodenvをインストール"
tags : [Node.js, JavaScript]
date: 2022-02-14 20:14:20
---



備忘も兼ねて。

最近全然node触ってなかったが、久々に触ろうとして、
ndenvで最新nodeをいれようと思ったら、もうndenvが非推奨になってた。

代わりに [nodenv/nodenv](https://github.com/nodenv/nodenv) を使えとのこと。

せっかくなので、これ系のツールをまとめる [anyenv/anyenv](https://github.com/anyenv/anyenv) もいれてみた






### ■環境

* Ubuntu 20.04 LTS



### ndenvをアンイストール

ndenvでつかっていた、各フォルダの .node-version ファイルそのまま使えるというので、削除するだけ。

```bash
$ rm -rf ~/.ndenv
```





### anyenvのインストール

anyenvのインストール
公式のREADMEにある通りだけど、
僕は、.bash_profileではなく、.profileをつかっている。
これのせいで、 exec $SHELL -l をしても入り直したことにならないので、いつか直すかな。


```bash
$ git clone https://github.com/riywo/ndenv ~/.ndenv
$ echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.profile
$ ~/.anyenv/bin/anyenv init
```

ここで、PCをログインし直し。
すると、
ANYENV_DEFINITION_ROOT(/Users/riywo/.config/anyenv/anyenv-install) doesn't exist. You can initialize it by:
> anyenv install --init

というメッセージがでるので、

```bash
$ anyenv install --init
```

で、anyenvは完了



### nodenvのインストール

anyenvのおかげで楽になった。

```bash
$ anyenv install nodenv
```

ここで、ログインし直し。（またかｗ）

では、node最新いれよう。


```bash
$ ndenv install 17.5.0
$ ndenv rehash
$ ndenv global 17.5.0
```


これでいれると、npmのバージョンが少し古く、
最初にnpm使ったときにアップグレードしろって出るので、
アップグレードしておく。


```bash
$ npm install -g npm
$ npm --version
8.5.0
```

完了！

この後、pyenvもいれた。
特に問題なく使えている。




