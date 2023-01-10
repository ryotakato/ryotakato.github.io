---
layout: post
title: "Ubuntu 22.04 LTS のクリーンインストール後にした環境構築"
tags : [Linux, 環境構築]
date: 2022-11-27 18:21:44
---

[ここ](/2022/11/24/failed-to-update-ubuntu-2204) で書いた通り、Ubuntu 22.04 LTS をクリーンインストールしたので、
その後やったことなどまとめ

ちなみに、Ubuntu 22.04 LTS では、GUIがWaylandになっているが、なかなか使いやすい。


### ClamTK （ウィルス対策）

aptでは最新版がでてないので、debを直接インストール

```bash
$ wget -P ~/Applications/ https://github.com/dave-theunsub/clamtk/releases/download/v6.14/clamtk_6.14-1_all.deb

$ sudo apt install ~/Applications/clamtk_6.14-1_all.deb
```

もちろんインストール後にウィルスチェックと定義ファイルの自動更新をやった


### NVIDIAドライバーインストール

(2023/01/10 追記)
NVIDIAのGPU GTX 1050tiを使っているが、
今更ながら、ドライバーをインストールしていないことに気づいたので、いれておく。

```bash
# ドライバーがあるか調べる
$ ubuntu-drivers devices


# ドライバーインストール 終わったら再起動
$ sudo ubuntu-drivers install

```


### Chrome

ブラウザはChrome派

```bash
$ wget -P ~/Applications/ https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

$ sudo apt install ~/Applications/google-chrome-stable_current_amd64.deb
```




### Git 


```bash
# Git最新版をいれるためaptのリポジトリ追加
$ sudo add-apt-repository ppa:git-core/ppa
$ sudo apt update

$ sudo apt install git
$ git version
git version 2.38.1
```

ちなみに、Githubのssh keyはssh-keygenで再度作成して登録し直しておいた。



### Mozc （日本語入力）

入力メソッドとしては、fcitxを使っているので、そこにMozcをいれる

```bash
$ sudo apt install fcitx-mozc

```

で、僕は自作キーボード使っているから、
下記を参考にして切り替えを容易にできるようにしている。

[Linuxで尊師スタイルの環境構築 - Qiita](https://qiita.com/MTfirst/items/1b09a7631639122697a1)


```bash
$ git clone git@github.com:ryotakato/Sonshi-Changer.git ~/develop/keyboard/

# シンボリックリンク作成
$ ln -s ~/develop/keyboard/Sonshi-Changer/sonshi.desktop ~/Desktop/sonshi.desktop

# この後にデスクトップにて、sonshi.desktopを右クリックし、Allow Launcherをクリック

```

なお、一度、fcitxの設定画面にて、
MozcのKeyboard Layout をEnglishやJapaneseにしておかないと、Sonshi-Changerで英数と日本語入力を切り替えられない
これはどういうことかというと、
Sonshi-Changerの中で、
~/.config/fcitx/data/layout_override
をいじっているが、ただsedしているだけなので、このファイルが存在していないといけないということ。




### asdf

今年の2月に下記でanyenvをいれたが、
どうやらもうanyenvは流行ってないらしい。

[anyenvとnodenvをインストール](/2022/02/14/install-anyenv-and-nodenv)


代わりに今はasdfが流行っているそうだ。
anyenvの疎結合な作りは結構好きだったし、gaucheの環境構築 gauenvを作るぐらい、
〜env系は好きだったが、もう開発がほとんど進んでいないならしょうがない。

それに、最近のJS処理系のDenoを使ってみたかったが、それはasdfしかなかった。

インストールは下記に従う

[Getting Started &#124; asdf](https://asdf-vm.com/guide/getting-started.html)


```bash
# Gitでインストール
$ git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.10.2

# .bashrcに下記を追加
# source $HOME/.asdf/asdf.sh
# source $HOME/.asdf/completions/asdf.bash
$ vim .bashrc

```

ここで一旦ログインしなおし。(本当はシェルの再起動だけでよさそうだけど


asdfは、言語を追加するにはまずプラグインを追加しないといけない。
ここでは、PythonとGaucheとDenoの3つを追加

```bash
# プラグイン追加
$ asdf plugin add python
$ asdf plugin add gauche
$ asdf plugin add deno

# インストールできるpythonの確認
$ asdf list all python

# インストールできるgaucheの確認(jqが必要なので入れている
$ sudo apt install jq
$ asdf list all gauche

# インストールできるdenoの確認
$ asdf list all deno
```



### Pythonインストールと、NeoVim用のPythonをvenvで準備

Pythonをいれる 最新版指定 この時点では3.11.0

```bash
$ asdf install python latest
$ asdf global python latest

# pipを最新化しておく
$ pip install --upgrade pip

# reshimしておく
$ asdf reshim
```


次に後でインストールするNeoVim用のPython環境を整えるため、
ディレクトリを作って、venvを設定。
pipでpynvimをいれておく

```bash

# ディレクトリ作成
$ mkdir ~/nvim_python3 
$ cd nvim_python3/

# venv準備して、pynvimをいれる
$ python -m venv .venv
$ source .venv/bin/activate
$ pip install --upgrade pip
$ pip install pynvim
$ deactivate 

```

### NeoVim 

とりあえず最新版でないといけないほどの機能は使っていないので、普通にaptで。

```bash
# sudo apt install neovim

# .profileに下記もくわえておくconfigの場所をneovimに明示するため。
# export XDG_CONFIG_HOME=$HOME/.config
# export XDG_CACHE_HOME=$HOME/.cache
$ vim .profile

```



次に、いつものおきまりの設定を持ってくる

```bash
$ git clone git@github.com:ryotakato/dotfiles.git ~/dotfiles

# 下記を追記する
# let g:python3_host_prog = expand('~/nvim_python3/.venv/bin/python3')
$ vim dotfiles/neovim/init.vim 

# シンボリックリンクを貼る
$ ln -s ~/dotfiles/neovim ~/.config/nvim

```




### GaucheとDenoのインストール

特別なことはなし。

```bash
$ asdf install gauche latest
$ asdf global gauche latest
$ asdf reshim
$ gosh -V
Gauche scheme shell, version 0.9.12 [utf-8,pthreads], x86_64-pc-linux-gnu
```


```bash
$ asdf install deno latest
$ asdf global deno latest
$ asdf reshim
$ deno -V
deno 1.28.0
```



### プリンター準備

プリンターは Brother DCP-J952N を使っている。
年賀状の準備も近いので、使えるようにしておきたい。

初期でも認識はするが適切なドライバーが入っていないので、印刷ができない。

[ソフトウェアダウンロード &#124; DCP-J952N-B/W/ECO &#124; 日本 &#124; ブラザー](https://support.brother.co.jp/j/b/downloadlist.aspx?c=jp&lang=ja&prod=dcpj952n&os=128)

ここから、プリンタードライバー　（Linux）の2つをダウンロードし、aptでいれる。
LPRはいらないかもだけど、とりあえず念の為。


```bash
$ sudo apt install ~/Applications/dcpj952nlpr-3.0.0-1.i386.deb
$ sudo apt install ~/Applicationsdcpj952ncupswrapper-3.0.0-1.i386.deb
```

これどcupsでプリンターが認識され、印刷ができるようになった。

スキャンについては、昔書いた下記を参考に設定。

[Tavi's Travelog - Ubuntuで、Brotherプリンターを使ってスキャンできるようにするまで](/2021/03/29/scanner-on-linux)



### その他いれたもの


```bash
# 画像表示ソフト
$ sudo apt install mcomix

# 動画再生ソフト
$ sudo apt install vlc

# Steam
$ sudo apt install steam

# ImageMagick
$ sudo apt install imagemagick

# nmap
$ sudo apt install nmap

# Minecraft (aptがないので、snapでいれる
$ sudo snap install mc-installer

```









