---
layout: post
title: "「The Rust Programming Language」でRustを学ぶ その07 簡易Grepを作る(12章)のと、RustでのDIを調べた"
tags : [Rust]
date: 2025-10-09 13:30:44
---



[The Rust Programming Language 日本語版 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/title-page.html)

の、今回は12章「入出力プロジェクト: コマンドラインプログラムを構築する」

ソースは下記のminigrepディレクトリ
[ryotakato/rust_the_book_src](https://github.com/ryotakato/rust_the_book_src)


ただ、1ファイルの中から文言を探すだけの簡易Grepだけど、楽しかった。
Rustでのテスト駆動開発が学べたし。

ただ、この過程で、
Rustでモックってどうするんだろうって思ったところから、
DIはどんな感じでやるのかなって疑問が浮かび調べたところ、
色々と先人がいた。


[RustのDI &#124; κeenのHappy Hacκing Blog](https://keens.github.io/blog/2017/12/01/rustnodi/)
[Rust で DI &#124; blog.ojisan.io](https://blog.ojisan.io/rust-di/)
[Rust で DI する時の小技 · ryym.log](https://ryym.tokyo/posts/rust-di/)
[Rust の DI を考える –– Part 2: Rust における DI の手法の整理 - paild tech blog](https://techblog.paild.co.jp/entry/2023/06/12/170637)




Cake Patternというのは初めて知った。
ボイラーテンプレートが多そうでちょっと億劫だけど、
Rustのマクロで楽できるみたいなので、19章のマクロへ行ったらやってみたいと思う。

おそらくここまでが基本的な部分で、
次からは、もっと細かく、かつ複雑な内容になっていくと思う。



