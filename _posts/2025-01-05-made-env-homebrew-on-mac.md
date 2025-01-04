---
layout: post
title: "Macにて環境構築(Homebrewなど)"
tags : [Mac, 環境構築]
date: 2025-01-05 10:22:44
---


Macで環境構築をしたのは去年の8月だが、書いてなかったのに気付いたので、残しておく。


### 環境情報

まずは環境情報。
MacBookAir の、2023 M2 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		14.6.1
BuildVersion:		23G93
```


### Homebrewインストール



インストール方法としては、下記もあるが、これは使わない。
```bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

代わりに、Macはpkgがあるので、そっちを使う


先に下記でmacのCLIを追加
```bash
$ xcode-select --install
```

pkgを下記からダウンロードしてインストール
https://github.com/Homebrew/brew/releases/tag/4.3.16


インストールの最後に、PATHの追加が出る。
Apple M2なので、.bash_profileに下記を追加する。
なお、僕はzshではなく未だにbash派

```
eval "$(/opt/homebrew/bin/brew shellenv)"
```


ちなみに、久々のMacなので、下記がかなり参考になった

https://qiita.com/ko1nksm/items/59c2e8a7afa969af8212




```bash
$ brew --version
Homebrew 4.3.16
```


### 各種アプリケーションインストール

僕が必要とする基本的なところ。

```bash
# Mac App Store
$ brew install mas

# NeoVim
$ brew install neovim

# Git
$ brew install git
```

なお、Gitは、下記のSSHの設定もやった。

https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

ちゃんとパスフレーズも設定して、キーチェインに登録済み


コンソールとしては、最近流行ってきているらしいWarpを使う。

```bash
$ brew install --cask warp
```

https://docs.warp.dev/appearance/custom-themes
テーマをダウンロードして、wombatに変更
欠点として、日本語入力が確定するまで表示されないという問題があった。
対策としてGoogle日本語入力をいれる

しかし、

```bash
$ brew install google-japanese-ime
```

でインストールできなかったので、ブラウザからdmgをダウンロード
そして、一度ユーザーをログアウトして入り直すと、入力が変更できるようになった。
これでWarpでも日本語入力が変換前に表示がされるようになった。いちいちタブで変換を確定させないといけないのは微妙だが。


次にasdf
前にUbuntuでもいれたので馴染がある。

```bash
# asdfの前提ライブラリ
$ brew install coreutils curl

# asdf
$ brew install asdf

# asdfのための設定
$ echo -e "\n. \"$(brew --prefix asdf)/libexec/asdf.sh\"" >> ~/.bash_profile
$ echo -e "\n. \"$(brew --prefix asdf)/etc/bash_completion.d/asdf.bash\"" >> ~/.bash_profile
```


次はPythonと、neovimのための設定だが、
[Ubuntu 22.04 LTS のクリーンインストール後にした環境構築](/2022/11/27/made-env-after-ubuntu2204-install)
の、
「Pythonインストールと、NeoVim用のPythonをvenvで準備」と同じ


で、NeoVim使っていたらIMEが自動でOFFにならないのが困ったので、
IMEの自動OFFのためにim-selectをインストール

```bash
$ brew tap daipeihust/tap
$ brew install im-select
```

なお、neovim のプラグイン im-select.nvim もインストールしておいた。





その他

```bash
# ImageMagick
$ brew install imagemagick

# Zoom
$ brew install zoom

# Amazon Corretto (Java)
$ brew install --cask corretto

# Minecraft Launcher
$ brew install --cask minecraft

```

