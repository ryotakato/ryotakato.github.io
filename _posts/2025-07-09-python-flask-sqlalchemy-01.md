---
layout: post
title: "PythonのFlaskとSQLAlchemyの構成 その1"
tags : [Python]
date: 2025-07-09 11:00:44
---

あまり体系だっていないが、メモ用。
FlaskとSQLAlchemyを使って、Webアプリケーションを作る場合の構成について。

venv環境を作った後のライブラリインストール

```bash
$ pip install flask sqlalchemy Flask-Sqlalchemy
```

クライアントサイドはReactとかがいいのだが、一旦今はFlaskのjinja2上でtemplateからhtmlを生成し返却ようにしている。
DBはSQLiteなので特に何も他にインストールしなくても、標準でPythonに入っている。

SQLAlchemyだけを検索すると、微妙に違うのだが、
Flask-Sqlalchemyは、FlaskからSQLAlchemyを簡単に使えるライブラリなので、こっちを検索したほうが情報が見つかる。
その中で、テーブル間のリレーションに関して有用なURLを貼っておく。
1対1, 1対N, N対NのそれぞれでどうModelを作るか、N対Nで付加情報があった場合にどうするかをまとめてある。

[Flask-SQLAlchemy Relationships: Exploring Relationship Associations - DEV Community](https://dev.to/freddiemazzilli/flask-sqlalchemy-relationships-exploring-relationship-associations-igo)
[今更ながらFastAPI + SQLAlchemy でテーブルリレーションの実装まとめ（多対多） #sqlalchemy - Qiita](https://qiita.com/kenmaro/items/64d65ddc94499993c843)


ちなみに、SQLAlchemyのリレーションでは、back_populatesとbackrefがあり、書き方の違いは下記。
なお、backrefのほうが手軽だが、後での保守の容易さからするとback_populatesのほうが良いらしい。

[SQLAlchemyのback_populatesとbackrefの違いとどちらも使わない場合 #Python - Qiita](https://qiita.com/1234224576/items/ba66838b32b99cce51d2)


チュートリアル的な記事は下記。
[【Python Flask】初心者プログラマーのWebアプリ#1 簡単なページ作成 #Python3 - Qiita](https://qiita.com/Bashi50/items/30065e8f54f7e8038323)



なお、デザイン的なものは、Bootstrapを使って楽をする。
Flaskには、Bootstrapと連携するための色んなライブラリがあるが、
正直どれがメンテナンスされているか不明なのと、
どうせCSSとJSなので、CDNから自前で持ってくるほうが依存関係がないのでよいと思う。

ということで、
Bootstrapの公式サイトから、CDNのURLを持ってきて、それをtemplateに組み込む。
なお、現時点ではBootstrapは5.3.7だった。

[Bootstrap · The most popular HTML, CSS, and JS library in the world.](https://getbootstrap.com/)


一旦ここまで。

