---
layout: post
title: "Mac M4 にて環境構築(Homebrewなど)"
tags : [Mac, 環境構築]
date: 2025-07-24 22:53:44
---

[前回](/2025/01/05/made-env-homebrew-on-mac)の環境構築の記事を書いたのは1月だが、
新しい学校へ行く関係でMacBook Airの2025モデル(CPUはM4)を買ったので、
再度環境構築。
前から少し変わっているところも多い。



### 環境情報

まずは環境情報。
MacBookAir の、2025 M4 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		15.5
BuildVersion:		24F74
```


### Homebrewインストール



インストール方法は前と同じくpkgでいく

先に下記でmacのCLIを追加
```bash
$ xcode-select --install
```

pkgを下記からダウンロードしてインストール
[Release 4.5.11 · Homebrew/brew](https://github.com/Homebrew/brew/releases/tag/4.5.11)


インストールの最後に、PATHの追加が出る。
Apple M4なので、.bash_profileに下記を追加する。
なお、僕はインタラクティブシェルはzshではなく未だにbash派

```
eval "$(/opt/homebrew/bin/brew shellenv)"
```



```bash
$ brew --version
Homebrew 4.5.11
```




### 各種アプリケーションインストール

僕が必要とする基本的なところ。


#### Git

```bash
$ brew install git
```

なお、Gitは、下記のSSHの設定もやった。(もちろん、メールアドレスやユーザー名の設定も)

[新しい SSH キーを生成して ssh-agent に追加する - GitHub Docs](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

ちゃんとパスフレーズも設定して、キーチェインに登録済み
前のMacと同じSSHキーは使わず、改めて登録
流れはこんな感じ


```bash
# SSHキーの作成 ファイルはid_ed25519_githubとしておく
$ ssh-keygen -t ed25519 -C "メールアドレス"

# クリップボードにコピーしてGithubのサイトでSSHキーの公開鍵を登録
$ cat ~/.ssh/id_ed25519_github.pub | pbcopy

# ssh-agentの起動
$ eval "$(ssh-agent -s)"

# Macのキーチェインに登録
$ ssh-add --apple-use-keychain ~/.ssh/id_ed25519_github

# 試し
$ ssh -T git@github.com
```


#### Warp

```bash
$ brew install --cask warp
```

WarpでテーマをWombatにするために、
Githubから取得

```bash
$ mkdir -p $HOME/.warp
$ cd $HOME/.warp/
$ git clone https://github.com/warpdotdev/themes.git

```
WarpのSettingsからWombatに変更


今回のMacは英語版なんだけど、
日本語入力は必要なのでいれていく。
ここ、前はbrewでできなかったけど、今回はできた。



```bash
# Rossetta 2 にしておく
$ softwareupdate --install-rosetta

# Google日本語入力をいれる
$ brew install --cask google-japanese-ime
```

一応ここで再起動しておく。



#### asdf

次にasdfなんだけど、0.16で大きく変わったので、初期設定が少し変わっている。

```bash
# asdfの前提ライブラリ
$ brew install coreutils curl

# asdf
$ brew install asdf

# asdfのための設定
# .bash_profileに下記を記載
export PATH="${ASDF_DATA_DIR:-$HOME/.asdf}/shims:$PATH"
# .bashrcに下記を記載
. <(asdf completion bash)
```


#### Python

次はPython
[Ubuntu 22.04 LTS のクリーンインストール後にした環境構築](/2022/11/27/made-env-after-ubuntu2204-install)
の、
「Pythonインストールと、NeoVim用のPythonをvenvで準備」と同じ。
ただし、変わっているところも多いので、改めて。

まず、asdfでpythonのインストール

```bash
# python
$ asdf plugin add python
$ asdf install python 3.13.5
$ asdf set -u python 3.13.5
$ asdf reshim
```

#### NeoVim

まずはインストール

```bash
# NeoVim
$ brew install neovim
```



次にnvimで使うPython仮想環境の用意

```bash
# ディレクトリ作成
$ mkdir ~/nvim_python3 
$ cd ~/nvim_python3/

# venv準備して、pynvimをいれる
$ python -m venv .venv
$ source .venv/bin/activate
$ pip install --upgrade pip
$ pip install pynvim
$ deactivate 
```

上記環境へのパスはinit.vimに記載していて、
自分の設定ファイル系をまとめたリポジトリである、
[ryotakato/dotfiles](https://github.com/ryotakato/dotfiles)
をgit cloneして、
~/.config/nvimが上記内のneovimディレクトリを指すようにシンボリックリンクを作成

前と同じくim-select.nvimというプラグイン使っているのだが、
このプラグインに必要なbinaryがim-selectからmacismに変わってたのでインストール

```bash
$ brew tap laishulu/homebrew
$ brew install macism
```


#### その他プログラミング言語環境



asdfでは、nvimでも使うdenoや、Minecraftのためのjava(Amazon Corretto)もいれる。
ちなみに、asdfでJavaをいれる場合、JAVA_HOMEを設定するのが良いみたいだが、
今回僕はminecraftでしかJava使わないと思うので、一旦設定なしでいく。
必要になったら追加しよう。

```bash
# deno
$ asdf plugin add deno
$ asdf install deno latest
$ asdf set -u deno latest
$ asdf reshim

# Java
$ asdf plugin add java
$ asdf install java corretto-24.0.2.12.1
$ asdf set -u java corretto-24.0.2.12.1
$ asdf reshim

# Rust
$ asdf plugin add rust
$ asdf install rust latest
$ asdf set -u rust latest
$ asdf reshim

# Go
$ asdf plugin add golang
$ asdf install golang latest
$ asdf set -u golang latest
$ asdf reshim
```



#### MySQL

授業で必要になるみたいなので。



```bash
# インストール
$ brew install mysql


# バージョン確認
$ mysql --version
mysql  Ver 9.3.0 for macos15.2 on arm64 (Homebrew)

# プロセス起動
$ brew services start mysql
==> Successfully started `mysql` (label: homebrew.mxcl.mysql)

# 接続確認(quit;で抜ける)
$ mysql -u root


# プロセス停止
$ brew services stop mysql
Stopping `mysql`... (might take a while)
==> Successfully stopped `mysql` (label: homebrew.mxcl.mysql)
```

本当はrootのパスワード変えたり、ユーザー作ったりしたほうがいいんだけど、一旦後回し
-> やった。

```bash
# ログイン
$ mysql -u root

# rootユーザーのパスワード変更
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'ここに新規パスワード';

# testユーザー作成
mysql> CREATE USER 'test'@'localhost' IDENTIFIED BY 'ここにパスワード';


# 即時反映させる
mysql> flush privileges;

# 一度抜ける
mysql> quit;


# 再度ログイン(パスワードでログインできたらOK)
$ mysql -u root -p
```

MySQL Workbentchも必要なのでインストール

```bash
# インストール
$ brew install --cask mysqlworkbench

```




(2025/08/07 追記)
-> この数日後、mysql 9.4.0がbrewで利用できるようになっていたので、
下記で更新しておいた。

```bash
$ brew update

$ brew upgrade mysql
```






#### その他いれたもの。
KindleとかはAppStoreから直接落としたからここでは書かない。

```bash
# Mac App Store
$ brew install mas

# ImageMagick
$ brew install imagemagick

# Graphviz
$ brew install graphviz

# Zoom
$ brew install zoom

# Minecraft Launcher
$ brew install --cask minecraft

# Miro
$ brew install --cask miro

# Draw.io
$ brew install --cask drawio

# Figma
$ brew install --cask figma

# LM Studio
$ brew install --cask lm-studio

# Unity Hub とUnity
$ brew install --cask unity-hub
$ brew install --cask unity

# Docker関連
$ brew install --cask docker-desktop
$ brew install docker-compose

# yt-dlp
$ brew install yt-dlp

# Karabiner Elements (初回起動時、ANSI配列を選んで、必要なMacの設定を変更)
$ brew install --cask karabiner-elements
```


英語版Macで日本語入力に入力ソースを切り替えるときって、
デフォルトだとfnキー(地球儀マーク)を押すことで切り替えるんだけど、
今まで英数キーを使っていたから、この位置使いづらいな。
他のキー(Caps Lockとか)に変えるようかな。

(2025/08/01 追記) 結局、Macのキーボード設定から、Modifierキーの一覧のCapsLockキーをfnキーのGrove機能と同じになるようにした。
これで便利かなと思っているのだが、TABの下でaの横だから、結構落ち間違えが多い。
左手の小指に仕事をさせすぎている気がする。
ちょっともうちょいいい方法はないか模索する予定。


(2025/08/29 追記)
上記の設定だと、Grove機能がfnと共存しているせいで、
瞬時に日本語に切り替えて、「は」とか入力しようとしたときに、
fn+Hと認識されるので、全部開いているアプリがhideされるとかになって、
一々邪魔されるのが煩わしかったので、設定を戻して大人しくKarabiner-Elementsを入れる。
他の人が作ったルールなどを探してインストールもできるみたいだけど、
そうすると何やっているか分からなくて嫌なので、
自分で設定書いた。
やっていることは、左Cmdキーの単発押しでIME OFF、右Cmdキーの単発押しでIME ON、
あと、CapsLockキーはCtrlキーと同じ動きになるようにした。
海外の人みると、結構CapsLockを使っているみたいだけど、僕からすると邪魔でしかない。


参考にしたのは下記
[Karabiner-Elementsでcommand単体押しで英かなキーを送信するように設定するとcommand + クリックが効かなくなる問題の修正 – Webrandum](https://webrandum.net/karabiner-elements-command-click/)



```json
{
    "description": "caps_lock -> control, left_command -> IME OFF, right_command -> IME ON",
    "manipulators": [
        {
            "from": {
                "key_code": "caps_lock",
                "modifiers": { "optional": ["any"] }
            },
            "to": [
                {
                    "key_code": "left_control",
                    "modifiers": []
                }
            ],
            "type": "basic"
        },
        {
            "from": {
                "key_code": "left_command",
                "modifiers": { "optional": ["any"] }
            },
            "to": [
                {
                    "key_code": "left_command",
                    "lazy": true
                }
            ],
            "to_if_alone": [{ "key_code": "japanese_eisuu" }],
            "to_if_held_down": [{ "key_code": "left_command" }],
            "type": "basic"
        },
        {
            "from": {
                "key_code": "right_command",
                "modifiers": { "optional": ["any"] }
            },
            "to": [
                {
                    "key_code": "right_command",
                    "lazy": true
                }
            ],
            "to_if_alone": [{ "key_code": "japanese_kana" }],
            "to_if_held_down": [{ "key_code": "right_command" }],
            "type": "basic"
        }
    ]
}


(2025/11/10 追記)

MacのOSアップデートしたら、なんだかKarabiner-Elementsの動きが悪くて、
上記のアプリ切り替え(Command+Tab)がかなり遅くなった。

うーんと思って、試しに上記の設定のleft_command, right_commandのlazyをfalseにしてみたら動くようになった。
lazyだとCommand+Tabも効かなくなるのかなって思ったけど、そうではないらしい。
あまり詳しく調べてないけど、上手くいったのでまあいいや。



```



