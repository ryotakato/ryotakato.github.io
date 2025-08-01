---
layout: post
title: "ripgrepの導入と、Telescope.nvimを使ってみた"
tags : [Mac, 環境構築, Vim]
date: 2025-08-01 13:59:44
---

nvimでgrepを効率的にしたかった。
最近はripgrepをTelescope.nvimで使うのが流行っているみたいなのでやってみる。



### 環境情報

まずは環境情報。
MacBookAir の、2025 M4 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		15.5
BuildVersion:		24F74
```


### ripgrepのインストール


どうやら、Rust製らしいけど、それもインストールの決め手になった。

brewでインストールは容易

```bash
# インストール
$ brew install ripgrep

# バージョン確認
$ rg --version
ripgrep 14.1.1

features:+pcre2
simd(compile):+NEON
simd(runtime):+NEON

PCRE2 10.43 is available (JIT is available)
```





### Telescope.nvimのインストール


プレビューできる機能が最高。
設定はよく分からないけど、とりあえず使い始めてからにする。


僕は未だにdein.vimを使っているので、
dein.tomlに下記を追記
plenary.nvimは依存関係


```toml
[[plugins]]
repo = 'nvim-lua/plenary.nvim'

[[plugins]]
repo = 'nvim-telescope/telescope.nvim'
depends = ['nvim-lua/plenary.nvim']
```



これだけでインストールできて、
:Telescope live_grep
とか、
:Telescope find_files
とかが使えた。


ショートカットキーを導入したいけど、
そろそろdein.vimから他のプラグイン管理に変えたほうがいいかな。
設定とか探しても他のばっかり出るんだよね。






### その他


今回改めてNeoVimの最近を調べていたら、
色々情報があって、いい加減僕も自分のVim設定を見直してみるかという気になっている。
Vimに戻ってもっと編集能力を磨くべきか、
NeoVimでどっぷり浸かるべきか。

Vimをどうするかは悩んでいるけど、とりあえずlazygitは入れた。
yaziとかのファイルマネージャーもほしいなと思いつつ。


```bash
# LazyGit インストール
$ brew install lazygit

```





