---
layout: post
title: "MacにNeoVimをインストール pyenv使用"
tags : [vim]
date: 2019-01-27 11:32:44
---


そろそろNeoVimを本格的に使っていこうかと思い、Macにインストールしてみた。  
基本的なインストール方法はたくさんの人があげていると思うので、  
2019年1月時点、かつ、pyenvを使っているという状況での特殊例として。  



### 環境

* Mac OS X 10.13.6
* Git 2.17.2
* Homebrew 1.9.3
* pyenv で3.4.0のPythonをインストール済み


### NeoVimインストールまで


MacだとHomebrewで入れるのが一番簡単。  
ただ、更新していたのが結構前なので、更新しておく。  

```bash
$ brew update
・・・コマンド結果は省略・・・
$ brew install neovim
```

ここまでは特に困らずにすぐだった。  
あえていうなら、neovim入れる際に、gettextも入れているのだが、それが一番長かった。  
PCが古いのもあるが、gettextだけで20分。  



### Pythonインストールまで

NeoVimを使うのに必須ではないのだけど、  
ほぼ間違いなく入れた方がいい。  
pyenvを使っていたので、結構前に入れた3.4.0をglobalに設定してなんとかならないか試す。  

なお、NeoVimでPythonと連携するには、pipでpynvimが必要。  
このモジュールはこの前まで、neovimというモジュール名だった(0.3.2からpynvim)から間違えやすいので気をつける。  

```bash
$ pyenv global 3.4.0

$ pyenv rehash

$ pip install pynvim
```

ここで、エラー発生。  
`There was a problem confirming the ssl certificate: [SSL: TLSV1_ALERT_PROTOCOL_VERSION] tlsv1 alert protocol`
みたいなメッセージで、やっぱりちょっと古いのが問題かなと思った。  
このまま3.4.0で上記問題を解決してもいいのだけど、  
いうほど3.4.0を使っていたわけでもないので、これを機に新しいのをインストールする。  
pyenv使っているので、できなかったら最悪戻せばいいしね。  


```bash
$ pyenv install 3.7.2

$ pyenv global 3.7.2

$ pyenv rehash

$ pip install pynvim
```


これで、pip installもエラーがでずに完了。  
NeoVimを起動してみると、  
ちゃんとhas('python3')が有効になっていることが確認できた。  
なお、NeoVimだと、:checkhealthの方が色々チェックできて助かる。  

pyenvでPythonをインストールすると、  
NeoVimのプラグイン周りで問題が起きるという話も聞いているが、  
今のところ問題ない。  
このPCでは、pyenvでしかPythonを入れてないせいかもしれない。  



### おまけ NeoVimプラグイン

とりあえずNeoVimのinit.vimと、プラグインを晒す。  


[ryotakato/dotfiles](https://github.com/ryotakato/dotfiles)


neovimディレクトリに対して、  
~/.config/nvim  
というシンボリックリンクを貼れば良い。  
なお、colorschemeはwombat最強。異論は認める。  


NeoVimプラグインのプラグインマネージャーと言えば、deinなので、それで入れる。  
他はlightlineとか、foldCCとかを愛用している。  
まあこれからまた増えて行くかな。  


早速、Vaffleというファイルマネージャーがいい感じ。  
しばらく使ってみる。  










