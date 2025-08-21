---
layout: post
title: "「The Rust Programming Language」でRustを学ぶ その01 3章まで"
tags : [Rust]
date: 2025-08-21 18:00:44
---


あまり時間が取れないかもという不安はあって、ウジウジ悩んでいたけど、
悩むのに飽きたから、とりあえず飛び込んでみようと思って、Rust始めてみた。


Rustaceans (Rustプログラミング愛好家のこと) たちの間では、
Rustを始めるなら、これ！という 通称 "The book" と呼ばれる本が存在するらしいので、それでスタート。

[The Rust Programming Language 日本語版 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/title-page.html)



とりあえず3章までは終わらせた。


ソースは下記
[ryotakato/rust_the_book_src](https://github.com/ryotakato/rust_the_book_src)

hello_worldとhello_cargoが1章
guessing_gameが2章
variablesが3章


3章の最後に、

* 温度を華氏と摂氏で変換する。
* フィボナッチ数列のn番目を生成する。
* クリスマスキャロルの定番、"The Twelve Days of Christmas"の歌詞を、 曲の反復性を利用して出力する。

という課題があったので、それも書いてみた。
再帰関数もかけるのかなと思ってやってみたけど、問題なく動いた。

スコープの考え方が面白い。

今のところ他の言語と似た感じで、PythonやJava強めな文法だけど、考え方は関数型言語っぽいという印象。

この本終わったら、「Rustの練習帳 : コマンドラインツールの作成を通してRustを学ぶ」ってのをやってみたいし、
コンパイラとかプログラミング言語も作れたら嬉しいなって思ってる。
とりあえず今年の残りはRustを学ぶってことでいこうと思う。






