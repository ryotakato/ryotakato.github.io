---
layout: post
title: "「The Rust Programming Language」でRustを学ぶ その09 Cargo(14章)"
tags : [Rust]
date: 2025-10-13 11:38:44
---



[The Rust Programming Language 日本語版 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/title-page.html)

の、今回は14章「CargoとCrates.ioについてより詳しく」

ソースは下記の、adderディレクトリとminigrepディレクトリ(前回のを引き継いで使っている) けど、コメント追加したぐらい
[ryotakato/rust_the_book_src](https://github.com/ryotakato/rust_the_book_src)



前半のドキュメンテーションコメントのところで、
ドキュメントのExampleに書いたソースを自動で単体テストしてくれるのには驚いた。
これはかなりありがたいんだけど、
ちゃんと通るコメント書かないといけないってのが面倒に感じて、ignoreばっかりつけてしまうかも。

また、全部のコメントでパッケージをuseしておくか、minigrep::search のように呼び出さないといけないのは、面倒。
まあ、元々ライブラリとして公開するつもりのExampleなのだから当たり前だが。

"//!" で始まるコメントは、ファイルの一番上にかかないとコンパイルエラーになる。
そのファイルで使っているuseよりも前だから、気をつけないと。
これ、本書には書いてなかったけどね。

後半のCrates.ioや、ワークスペースの考え方などは、まあ実際に自分がライブラリ開発するときでいいかと思い、流し読み。

次はスマートポインタ

