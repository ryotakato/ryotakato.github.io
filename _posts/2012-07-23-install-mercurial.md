---
layout: post
title: "Mercurialインストール＆初期設定"
tags : [SCM, Mercurial]
date: 2012-07-23 00:27:18
---


分散バージョン管理（DVCS）は、Gitを今年から使っているけど、
二つ目として、Mercurialに手を出し始めました。
今回はそのインストールについて、書きます。

## ■環境

* Mac Book Air ( Lion )
* コマンドは全てbash
* Git インストール済み（Git 1.7.5.4）
* MacPortsインストール済み（MacPorts 2.1.1）

## ■手順

まず、mercurialをMacPortsからインストール。
ここで、bashの入力補完を効かせるように、`+bash_completion`をつける。

```bash
$ sudo port install mercurial +bash_completion
$ hg --version
```

でバージョンが表示されればOK
因みに、僕の環境では、version 2.2.2


次に、初期設定。
補完が効くように、.bashrcに

```bash
$ export BASH_COMPLETION_DIR=/opt/local/etc/bash_completion.d
$ source $BASH_COMPLETION_DIR/mercurial
```

を追加

おー、ちゃんとMercurialコマンドの入力補完が効く！！

.hgrcの設定だけど、
とりあえず最初は、usernameだけで十分かと。

次に、bashでGitのリポジトリにいるのか、
Mercurialのリポジトリにいるのか分かりやすくするため、
以下を参考に、プロンプトに各リポジトリのブランチ名を表示させてみた。

[bashでgitとmercurialを使いやすくする - YAMAGUCHI::weblog](http://ymotongpoo.hatenablog.com/entry/20110115/1295082759)

```bash
$ PS1="\n\[\033[1;32m\]\t\[\033[0;31m\]\$(git_branch)\$(hg_branch) \[\033[1;32m\]\$ \[\033[0m\]"$
```



うん、使いやすいね！
ってことで、これからばりばりMercurialユーザーだ！


