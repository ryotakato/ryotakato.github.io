---
layout: post
title: "Macのコマンドラインで画像を簡単に表示するには"
tags : [Mac]
date: 2013-03-24 22:09:09
---

Macを使ってて、ちょっと画像ファイルを見るのに、
いちいちFinderを開いて、該当のファイルを探してプレビュー.appで開くとか面倒だと思った。
特に、プログラムから画像ファイルを生成するような場合。

そこで、Macでコマンドラインから画像ファイルを開くことはできないかなと思い探してみると、
以下の三つの方法がある。

1. display [ファイル名] ※ImageMagick必須
2. open -a Preview [ファイル名]
3. plmanage -p [ファイル名]

1は、ImageMagickを入れればフォーマットの変換などできるので、色々便利かもしれないが、
ただ表示するだけにはオーバースペック
しかもインストールに時間かかるしね。

2は、ただ単にプレビュー.appをコマンドから開いているだけ。
ただし、僕はプレビュー.appをもっぱらPDF用としてしか使用しておらず、
いつでも全画面で表示されるようにしているので、
コマンドラインからの画面切り替えがゆっくり過ぎてイライラしたので却下。


3は、今まで知らなかったけど、qlmanageというコマンドがSnowLeopard時代からあったようだ。
これはFinderで、画像ファイルを選択したときにFinder内で小さくプレビューされる機能のコマンドライン版らしい。
こりゃ、一番手軽だ！



ということで、3でいくことにした。
ただ、コマンドの出力結果は虚空の彼方に消し去ってほしいので、
~/.bashrcにaliasを定義しておこう

```bash
alias ql='qlmanage -p "$@" >& /dev/null'
```


