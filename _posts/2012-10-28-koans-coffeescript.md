---
layout: post
title: "CoffeeScriptを公案で学んでみた"
tags : [CoffeeScript, JavaScript]
date: 2012-10-28 00:09:03+09:00
---



CoffeeScriptは以前から気になっていた。
JavaScriptを簡潔に書けるというのが、面白そうだったからだ。
だけど、別にJavaScriptにそんなに不便を感じてなかったから、手を出してはいなかった。

しかし、この記事を見て、ちょっと面白そうだと思い、手を出してみた。
[禅の公安スタイルでCoffeeScriptを学ぶ「coffeescript-koans」 - にのせき日記](http://d.hatena.ne.jp/ninoseki/20111023/1319369396)

プログラミングの世界での公案とは、まあただの穴埋め問題なわけだけど、
知らない人からすれば、禅の公案のようにちんぷんかんぷんなので、「公案」と呼ぶのだろうか。
とりあえず、CoffeeScriptを導入して、「coffeescript-koans」をやってみるところまで。

## ■環境


* Mac Book Air ( Lion )
* シェル:bash
* Node.js インストール済み（v0.8.2）
* Git インストール済み（Git 1.7.5.4）



## ■導入


CoffeeScript自体はとても簡単


```bash
sudo npm install -g coffee-script
```



なお、僕の環境ではsudoをつけたが、
管理者権限がある場合などは不要だと思う。
sudoなしでやってみて、エラーが出たら、sudoつければいい。


次に、「coffeescript-koans」

適当なディレクトリで、

```bash
git clone git://github.com/sleepyfox/coffeescript-koans.git
```


これだけ
やっぱ、Github便利だわ。



## ■回答（実装）


coffeescript-koansというディレクトリができているので、そこに移動する。

koansディレクトリ配下にAbout〜.coffeeというファイルがあるので、これを開き、
各CoffeeScriptの`FILL_ME_IN`という文字に該当する答えを書き込んでいく。

ファイルを書き換えるので、masterブランチは使いたくない。
僕はGitでanswerというブランチを作り、そこで自分の答えを書いてコミットしていっている。

なお、JavaScriptしかしらない人がいきなりこれに手をつけても何のことやらって感じなので、
以下のサイトを一度読んでおくといいと思われる。（できれば多少自分で動かしてみたほうがよい）
[今日から始めるCoffeeScript | tech.kayac.com - KAYAC engineers' blog](http://tech.kayac.com/archive/coffeescript-tutorial.html)


## ■答え合わせ（テスト）


「coffeescript-koans」は、テストフレームワークとして、Jasmineを使っている。
Jasmineの詳細な説明は長くなるので、ググレカス状態だが、
簡単にいうとJavaScriptのBDDテストフレームワークで、RubyのRSpecみたいらしい。
RSpecよく知らないので、詳しくは分からない。


さて、先ほど書いた穴埋めが正しいかどうかテストしてみる。
手順としては、

1. CoffeeScriptをビルドしてJavaScriptファイルを生成する
2. HTMLを開くことでJasmineがテストを実行し、結果が表示される

だ。





1のやり方は


```bash
cake build
```


というのが簡単だけど、
CakeFileの対象は全てのCoffeeScriptなので、まだ全部回答していない場合は、
未回答の分が全てテストエラーで赤くなる。

別にそれでもかまわないという人はいいのだけれど、
僕はテスト対象は絞りたい派なので、


```bash
coffee -c -o lib/koans/ koans/AboutFunction.coffee
```


のように一部のファイルだけコンパイルするようにしている。


1が終わったら、テスト結果を見るために2を実行しよう。
ただ単純にKoansRunner.htmlをブラウザで開くだけだ。
推奨ブラウザはみんな大好きGoogle Chrome


さて、結果が表示されたな。
よしよし、どれどれ・・・。


うぉおう！？真っ赤やないですか！！

ってことで、テストが失敗したら、どこか間違っている証拠。
また修正して同じ手順を繰り返すわけだ。


これで、あなたもCoffeeScriptを始めた！と胸を張って言えるね！


## ■感想


CoffeeScript楽しいね！
短いので、書くのが楽ってこともあるけど、
それより、簡潔なおかげで読みやすい。

だけど、厳密なアプリケーション書くときは誤記とかで簡単なバグにハマりそうだなw


あと、僕はVimmerなんだけど、Vim使いの人はプラグインの「vim-coffee-script」を入れたほうがいいよ。
全然書きやすさが違う。
あと、実行するときは「quickrun」もおススメ。
[VimでCoffeeScriptを書く準備 - いろいろな何か](http://d.hatena.ne.jp/yogit/20110710/1310269515)
まあ、Vimのことはまた今度。

