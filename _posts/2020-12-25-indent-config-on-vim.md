---
layout: post
title: "Vimでのc言語とMakefileのインデント設定"
tags : [Vim]
date: 2020-12-25 23:54:44
---

備忘も兼ねて。  



普段Vim(NeoVim)を使っているので、
インデントの設定はinit.vimに書いている。
それがこれ

```vim
set tabstop=4
set expandtab
set shiftwidth=4
set autoindent
```

特にファイルタイプを指定せずに設定していて、
タブはスペース4つに変換するようにしている。

基本的にこれで困ることは少ないのだけど、
最近C言語をMakefileでビルドすることがあって、
Makefileはタブじゃないといけなかったので、困った。
勝手にスペースに置き換えないでほしかったのだ。

ということで、
~/.config/nvim/after/indent/
の下に各ファイルタイプ毎の設定ファイルを用意する。


まずC言語(c.vim)

```vim
setlocal expandtab
setlocal tabstop=2
setlocal shiftwidth=0
setlocal softtabstop=0
setlocal autoindent
```

次にMakefile(make.vim)

```vim
setlocal noexpandtab
setlocal tabstop=8
setlocal shiftwidth=0
setlocal softtabstop=0
setlocal autoindent
```

C言語はあまりインデントを深くしたくなかったので、2にした。
そして、Makefileはタブのスペース置換をやめた。

afterの配下に置いたのは、
今後プラグインを追加したときでも上書きされないようにするため。



各オプションの意味など、下記がとても参考になった。

[Vim の 5 つのタブとスペース関連のややややこしいオプションをできるだけ簡単に解説するよ - yu8mada](https://yu8mada.com/2018/08/26/i-ll-explain-vim-s-5-tab-and-space-related-somewhat-complicated-options-as-simply-as-possible/)


