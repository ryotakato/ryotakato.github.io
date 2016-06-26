---
layout: post
title: "Backbone + Marionette を Yeomanで使う(と、MarionetteのUpgradeでハマった話)"
tags : [JavaScript,Backbone,Marionette,Yeoman]
date: 2015-02-08 14:55:32
---




## はじめに


* Backboneとは、Javascriptのクライアントサイドのフレームワーク
* Marionetteとは、Backboneを使い易くするためのフレームワーク
* Yeomanとは、Node.jsのアプリケーションの開発サポートをする便利ツール

ふつう、Yeomanの場合、
Angular.jsを使うことが多いのだろうけど、
個人的にはBackboneのほうが好きなので、こっちで。

素のままのBackboneを使っても全然アプリ開発はできるのだけど、
最初のころは、どこに何を書いたらいいのかがよく分からないので、
Marionetteを使ったほうがいいと思う。
Backboneはわりと自由に書いても動くので、
どこに何を書けばいいのかわからないというより、どこに書くのがBest wayなのかがわからないって感じかな。

その分、Marionetteはある程度書く場所が決まっており、
お決まりのコードとかは自分で書かなくていいので、楽。

Backboneの勉強をしたいなら、先にMarionetteを導入しておいて、
だんだん分かってきたら、削るという選択肢を考えるのがいいと思う。
少なくても僕はそうした。
最初Backboneだけで、一度挫折したのは内緒w
勿論、そのままMarionetteを使い続けてもOK





## Yeoman


Yeomanは、

* yo
* Grunt
* Bower

の３つからなるツール
実際は、GruntとかBowerは別に使われているから、
yoがメインになるんだけど、
generatorというものを使って、
アプリケーションの雛形を自動的に作成してくれるもの。

一個一個アプリで使うライブラリを用意しなくていいのもあるけど、
それより、どういうファイル構成にしようかを考えなくて済むのは非常に楽。
勉強にもなるしね。



## generator-marionette


今回は、Marionette用のgeneratorを使って、アプリの雛形を作るのだけど、
Yeomanとか、Marionetteとか検索したら出てくるのがこのページだと思う。

[Marionette.js(Backbone.js)のチュートリアル with yeoman その１(準備からサーバー側実装まで) - lxyuma BLOG](http://lxyuma.hatenablog.com/entry/2013/10/04/001331)


僕もこのページには大変お世話になった。
今度リリース予定の今作ってるアプリも、これがあったからできたというもの。


で、上記のページでは、
[mrichard/generator-marionette](https://github.com/mrichard/generator-marionette)

を使っているのだけど、
現時点以降で使うなら、絶対こっちのほうがいい。

[nichdiekuh/generator-marionette2](https://github.com/nichdiekuh/generator-marionette2)

もうMarionetteは、バージョンが2.0を越えているので、
今から作るのに、わざわざ古い方を選ぶ必要はない。

それに、Marionetteの1.0系 -> 2.0系の変更がかなり色々手が入っていて、
あとでバージョンアップしようというのはかなり面倒。

具体的にいうと、
closeという言葉が全体的にdestoryに置き換えられていて、
関数名が変わってしまってる。。。。
しかも、CollectionViewのitemViewは、これまたchildViewという名称に変わっているので、
当然そのままでは動かない。


ただ、Upgradeするために、

[marionettejs/Marionette.Upgrade](https://github.com/marionettejs/Marionette.Upgrade)

というアップグレードスクリプト(要python)があったりする。
これを作ってくれたのは非常にたすかったのだが、
うまくいくように見せて、
itemView -> childView の変換が上手くいってなかったり
(issueあげました [Not replacement itemView -> childView · Issue #35 · marionettejs/Marionette.Upgrade](https://github.com/marionettejs/Marionette.Upgrade/issues/35) )
(2015/02/16 追記：ごめんなさい、僕の完全なる勘違いでした。でも、その代わり分かりづらいと思ったところをpull request 出しました)
するので、

使ったあとはしっかり動作確認しないとまともに動かない。
実際いくつかコードに修正をいれざるを得なかった。



なので、最初から

[nichdiekuh/generator-marionette2](https://github.com/nichdiekuh/generator-marionette2)


を使ったほうがいいよっていう、それだけの話(長いよ)





## 使い方


Yeomanは入ってる前提
カレントディレクトリは、mkdirしたばかりのアプリ作りたいとこ


```bash
$ npm install -g generator-marionette2
```

ここは-gじゃなくてもいいのだけど、
他にも使う予定なら、-gをつけて、グローバルにいれておいても問題ない。


で、次に

```bash
$ yo marionette

```


すると、おじさんがでて、
なんだか色々聞かれるので、
とりあえず、全部yesにするのでいいと思う。

* Express使うか？
* MongoDB使うか？
* Socket.io使うか？
* Baucis使うか？
* Bowerはどこにいれる？

ってことなんだけど、
でもね、最後の質問はひっかけだと思うよ。
僕が実際にやったときは、「全部Yesに決まってんだろ！」とばかりにYを連打してたら、
Bowerのcomponentsのインストール先が"Y"になってしまったwww

いや、.bowerrcに書かれるから、
あとでも変更はできるけど、
このgenerator使うと、いろんなとこに書かれるので、最初からちゃんと指定したほうがいいよ。

というか、何もかかずに、Enterを押せば、
デフォルトの、`bower_components` というディレクトリ使ってくれるから、
ほんとこれのほうがいい。
Bowerの解説とか見ると、だいたい`bower_components`だし。



さて、今日はここまで。







## それにしても


Backbone = 背骨
に、
Marionette = 操り人形

って、かなりイカしたなネーミングだよね。
考えたやつ、Goodjob!






