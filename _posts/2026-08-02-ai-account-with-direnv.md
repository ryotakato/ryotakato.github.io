---
layout: post
title: "仕事とプライベートのAIアカウントをdirenvで使い分ける"
tags : [環境構築, AI]
date: 2026-08-02 17:45:44
---



Claude CodeとCursorをプライベートでも仕事でも使っている。
で、1台のMacで使い分けるということをしているのだけど、
間違って使ってしまうと、会社のお金で個人のことをやってしまったり、
個人のアカウントの少ない利用枠があっという間に使い切ってしまうとかいうことが起きるので、
ちゃんと使い分けをしなきゃいけないと思っている。
もちろんセキュリティ的にも。


で、公式のみでは方法がないので、
今回はdirenvを使って使い分けることとする。
その方法を書いておく。


なお、対象は、Claude CodeとCursorのCLI。
Cursorのデスクトップアプリのほうはdirenvを使わずにMacのAutomatorでやった。

Claude CodeもCursorも、ブラウザ版はあまり使わないので今回の対象外。
ブラウザを別にするなりすればできると思う。



### 方針

今どっちのアカウントを使っているかということを意識し続けるのは難しいので、
作業しているディレクトリがどこなのかで、仕事かプライベートかのアカウントを自動的に使い分けるようにする。

そのため、まずはプライベートと仕事で開発するディレクトリを分ける。
これは別にどこに作ってもいいけど、片方がもう片方の配下ディレクトリとかにならないように、 同じ階層に作っておく。


```bash
# プライベート用開発ディレクトリルート
$ mkdir ~/develop_private

# 仕事用開発ディレクトリルート
$ mkdir ~/develop_work

```



### direnvインストール

次にdirenvをインストール。
これはディレクトリごとに環境変数を分けることができるツール
今回は環境変数を差し替えることでアカウントの使い分けを行う。


```bash
$ brew install direnv
```

インストールしたら、.bashrcに下記を設定するように書いてあると思うので、設定する。
なお、僕はbash派なので、zsh派の人は適宜読み替えて。

```
eval "$(direnv hook bash)"
export PATH="$HOME/bin:$PATH"

```


### .envrcを作成し、読み込み

direnvが読み込むためのファイルを空で作っておく。

```bash
# 空ファイル作成
$ touch ~/develop_private/.envrc
$ touch ~/develop_work/.envrc
```

さて、それぞれの `.envrc` の中身だけど、下記のようにしている。

プライベート用 `.envrc`
```bash
export CLAUDE_CONFIG_DIR="$HOME/.claude-private"
export CURSOR_CONFIG_DIR="$HOME/.cursor-private"
export CURSOR_DATA_DIR="$HOME/.cursor-private"
export CURSOR_API_KEY="$(security find-generic-password -s 'cursor-api-key-private' -w)"
```

仕事用 `.envrc`
```bash
export CLAUDE_CONFIG_DIR="$HOME/.claude-work"
export CURSOR_CONFIG_DIR="$HOME/.cursor-work"
export CURSOR_DATA_DIR="$HOME/.cursor-work"
export CURSOR_API_KEY="$(security find-generic-password -s 'cursor-api-key-work' -w)"
```

.xxx-private と .xxx-work と、それぞれのAIツールの設定ディレクトリを分け、direnvがそれぞれのディレクトリに入ったら、自動で環境変数を切り替えることで実現している。

Claude Codeは、設定ディレクトリ (通常 `~/.claude`) をCLAUDE_CONFIG_DIRで指定できるし、その中にアカウントの認証情報も入っているのでその環境変数一個だけを設定すればよい。

しかし、Cursor CLIは、認証情報は設定ディレクトリ (通常 `~/.cursor`)に入ってこないので、
それぞれ仕事とプライベートでAPIキーを発行して、それ経由で認証を通している。
APIキーの値自体はMacのKeychainに登録してあるので、`.envrc` に直書きしなくてもいいのが助かる。
ただし、最近のMacはKeychainアプリがDockからは見つからず、Spotlight窓で検索して開くことができた。


終わったら、
それぞれ新規ターミナルセッションを開き、

プライベート用
```bash
$ cd ~/develop_private
$ direnv allow
```

仕事用
```bash
$ cd ~/develop_private
$ direnv allow
```

という風にdirenvで `.envrc` をallowしておく。



### Claude CodeとCursorの設定ディレクトリを分割


最後、AIの設定ディレクトリを作る。
もし今まで ~/.claude や ~/.cursor を使っていて、引き継ぎたいなら下記ディレクトリ内に移動しておくとよいが、絶対パスが変わるので、スキルとかは気をつけたほうがいいと思う。

```bash
# Claude Code
$ mkdir ~/.claude-private
$ mkdir ~/.claude-work

# Cursor
$ mkdir ~/.cursor-private
$ mkdir ~/.cursor-work
```


### Claudeの初回認証

CursorはAPIキーなので、認証設定は終わっているが、
Claudeは初回認証をしないと。

まずはプライベート用アカウント認証

```bash
# プライベート用ディレクトリに移動。
$ cd ~/develop_private
# Claude実行
$ claude
# ログインがでるので、ブラウザでプライベート用アカウントにログイン。
```


次に仕事用アカウント認証

```bash
# 仕事用ディレクトリに移動。
$ cd ~/develop_work
# Claude実行
$ claude
# ログインがでるので、ブラウザで仕事用アカウントにログイン。
```





### 実際に使ってみる。

このdirenvを使う方法のいいところは、
develop_privateや、develop_work配下にディレクトリつくっても、
上記で設定したそれぞれの `.envrc` が効いてくれること。

なので、

```bash
$ cd ~/develop_private
$ mkdir aaaa
$ cd aaaa
$ cursor-agent
```

とかやって、aaaaディレクトリ内でclaudeやcursorを起動しても、ちゃんとプライベート用のアカウントとして動いてくれる。


仕事用なら、

```bash
$ cd ~/develop_work
$ mkdir -p project_xxxx/repo_zzzz
$ cd project_xxxx/repo_zzzz
$ claude
```

って感じ。

階層深くなったとしても、
rootディレクトリが、~/develop_privateや ~/develop_work のように `.envrc` を設定したディレクトリであれば問題ない。


仕事用アカウントが、複数あるなら、さらにdevelop_xxxディレクトリを作って同様に分ければよい。


もちろん、ちゃんとアカウント間違えていないかの確かめは自分でやること。
claude起動したあとに、/about とか打てば分かるはず。


ふいー、これで間違えて慌てることはなくなった。




### おまけ: Cursorデスクトップアプリの使い分け。

Cursorデスクトップは、起動したときの引数に user-data-dirを設定できる。(Electronだから？)
で、このディレクトリ内に認証情報が保存されるので、
MacのAutomatorを使って、プライベート用と仕事用で別々の起動アプリを作る。
設定する /bin/bash はこんな感じ。

```
open -na "Cursor" --args --user-data-dir="$HOME/.cursor-private"
```

まだ認証してなければ、初回起動後に認証が聞かれるので、ブラウザでそれぞれのアカウントで認証しておけば、
それ以降は基本的にアプリ立ち上げるだけで、使い分けができる。







