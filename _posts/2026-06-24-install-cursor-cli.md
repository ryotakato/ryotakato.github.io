---
layout: post
title: "Cursor CLIをインストールした"
tags : [環境構築, AI]
date: 2026-06-24 23:14:44
---



備忘録


Cursorを少し前から使っており、Cursorエディタにも慣れてきたのだけど、
別にCursorエディタを使いこなしているわけじゃないので、ターミナルからでいいんだよね。
だいたいCursorエディタの勝手にファイル開いたりする仕様、あまり好きじゃないし。


ということで、CursorのCLIをインストール

Homebrewもあるみたいだけど、
全然ヒットしないし、
更新が速いAI分野なので、公式が言っているやり方が正解だろうと思って、
今回はあえてhomebrewを選ばなかった。

[CLI Installation &#124; Cursor Docs](https://cursor.com/ja/docs/cli/installation)



```bash
$ curl https://cursor.com/install -fsS | bash
```

でインストールすると、
agentコマンドが使えるようになる。

```bash
$ agent --version
2026.06.24-00-45-58-9f61de7
```

にしても、コマンド名agentって。。。もうちょっとcursorであることを識別しやすいコマンドはなかったのか。
cursorコマンドはCursorエディタ用のコマンドなのは分かるけど、じゃあcursor-cliとか。
エイリアスでも登録しておくべきか？


とりあえず今はagentのままでいくと、
初回起動時にログイン確認を求められるので、ブラウザでログインして、セットアップは完了。



