---
layout: post
title: "「The Rust Programming Language」でRustを学ぶ その11 並行性(16章)"
tags : [Rust]
date: 2025-10-19 12:45:44
---



[The Rust Programming Language 日本語版 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/title-page.html)

の、今回は16章「恐れるな！並行性」

ソースは下記の、concurrencyディレクトリ
[ryotakato/rust_the_book_src](https://github.com/ryotakato/rust_the_book_src)



思ったより簡単そうにスレッド作ったり、
メッセージ送ったりできた。
しかも、スレッド越しで所有権送ることが普通なので、
それを回避したい場合にArcを使うことも分かった。

確かにRustの安全性は、こういう並行プログラミングを書くときにかなり本領を発揮するなと思った。
例えば、SendついていないRcをスレッドに送ることができなかったり。


次はオブジェクト指向。








