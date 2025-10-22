---
layout: post
title: "「The Rust Programming Language」でRustを学ぶ その12 オブジェクト指向(17章)"
tags : [Rust]
date: 2025-10-22 21:54:44
---



[The Rust Programming Language 日本語版 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/title-page.html)

の、今回は17章「Rustのオブジェクト指向プログラミング機能」

ソースは下記の、oopディレクトリ
[ryotakato/rust_the_book_src](https://github.com/ryotakato/rust_the_book_src)


オブジェクト指向は慣れているから別に難しくはないんだけど、
Rustで書くときの感じを学ぶので、 Boxとかトレイトとかを上手く使う必要があり、そこがちょっと苦労した。

特に、「17.3. オブジェクト指向デザインパターンを実装する」のところで、
説明しているソースコードが少し古く、
英語版のほうをみにいったら、

Box<State> ではなく、 Box<dyn State> と書かれていて、
このdynがないとコンパイルエラーになってしまう。

dynは、Rustの2018エディションから、入ってきたみたいで、
どうして必要なのかを厳密には理解していないけど、まあつけないとBoxでトレイトを定義できないのは分かった。

[新しいキーワード - エディションガイド](https://doc.rust-jp.rs/edition-guide/rust-2018/new-keywords.html)



あと面白かったのは同じ章のところで、
State Patternを、Rustの型安全性を活かしたまま別の形で実現してしまおうという試み。
確かにこのほうがシンプルに書けるなって感じで、
言語が変われば、デザパタをわざわざ書かないといけないかっていう必要性も変わってくるなと思った。
根本の問題は一緒なんだけど、それをどう実現するかで、
デザパタを使わなくても書けるよっていう示唆が面白い。



次はパターンマッチング。match文の複雑な使い方かな？








