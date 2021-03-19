---
layout: post
title: "herokuのstackを20にアップデート＆heroku自体も更新"
tags : [Linux]
date: 2021-03-19 09:39:44
---


Herokuのstackが16のままで、
いよいよアップデートしろよっていうメールがきたのでアップデートする。

ここに書いてあるのがすべて。

[Upgrading to the Latest Stack &#124; Heroku Dev Center](https://devcenter.heroku.com/articles/upgrading-to-the-latest-stack)


やったのは下記だけ。
アプリ名のところは適宜変更

```bash
# herokuのstackを20にアップデート
# これで次のデプロイでアップデートするようになる
$ heroku stack:set heroku-20 -a [アプリ名]

# デプロイのために空コミット
$ git commit --allow-empty -m "Upgrading to heroku-20"

# デプロイ
$ git push heroku master


```


そして今回久々にherokuコマンドたたいてみたら、
結構バージョン古くて、怒られたので、どうせなら更新する。

aptでいれてたので、upgradeするも、できない。
あれ？と思い、aptのsource.list.dをみると、
herokuの取得先パッケージが、
Ubuntu 20.04LTSへのアップデート時に無効化されていた。
有効化しようにも、取得先がもうなさそう。

公式的にももうsnapで扱うみたい。

なので、aptからはアンインストールしてsnapでいれなおす。



```bash
# aptからはアンインストール
$ sudo apt remove heroku

# snapで再インストール
$ sudo snap install --classic heroku


# バージョン確認
$ heroku version
 ›   Warning: heroku update available from 7.49.0 to 7.51.0.
heroku/7.49.0 linux-x64 node-v12.16.2
```

最新は7.51.0だが、snapのstableだとまだ7.49.0のよう。
まあ特にこだわりないのでこれでよしとする。








