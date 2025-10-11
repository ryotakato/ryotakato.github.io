---
layout: post
title: "「The Rust Programming Language」でRustを学ぶ その08 イテレータとクロージャ(13章)"
tags : [Rust]
date: 2025-10-11 16:50:44
---



[The Rust Programming Language 日本語版 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/title-page.html)

の、今回は13章「関数型言語の機能: イテレータとクロージャ」

ソースは下記の、minigrepディレクトリ(前回のを引き継いで使っている)
[ryotakato/rust_the_book_src](https://github.com/ryotakato/rust_the_book_src)



別に真新しくはない機能だけど、最近の言語だとほぼ必須のイテレータとクロージャ機能。

一番素晴らしいと思ったのは、
Iteratorトレイトを実装し、nextメソッド書くだけで楽にイテレータが追加できること。
まあ、Javaとかでも割とそうなんだけど、
なんとなく今回でトレイトの便利さが分かってきた。
(self_iterator.rsファイルでそれ書いてみた。NewTypeイディオムとかも併せて勉強した)

あと、minigrepをイテレータを使うように改造したせいで、
単体テストが通らなくなっていたから修正した。
本書の内容では、env::Argsを引数にしていたが、
それを単体テストで自前で作るのは厳しかったので、
Rustのドキュメント調べて、env::ArgsもIteratorを実装しているのだから、
Iteratorをトレイト境界にして、ジェネリックスで実装すればいいんじゃないって思って書いてみた。
単なるIteratorではだめで、その中のItemの型も指定しないといけなかったけど、一応できた。
これで単体テストも、StringのVectorをinto_iter関数でiteratorに変換すれば渡せたから良しとする(所有権は移ってしまったけど)



なんだろう、難しい言語なんだけど、段々と色がついてきて、分かるようになってきたのが嬉しい。

