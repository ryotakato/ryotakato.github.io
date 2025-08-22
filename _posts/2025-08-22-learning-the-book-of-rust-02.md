---
layout: post
title: "「The Rust Programming Language」でRustを学ぶ その02 所有権(4章まで)"
tags : [Rust]
date: 2025-08-22 23:03:44
---



[The Rust Programming Language 日本語版 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/title-page.html)

の、今回は4章「所有権」

ソースは下記のownershipディレクトリ
[ryotakato/rust_the_book_src](https://github.com/ryotakato/rust_the_book_src)



Rustというと一番に話題になる所有権の話。
前から少しは知っていたので、そんなに違和感はなく読めた。

が、読めるのと書けるのは違う。。。
ので、今後色々とぶつかる問題なんだろうなと思う。


それで、この中の章で、文字のスライスの話が出てきたので、
ふっと、Rustで文字列結合はどういう書き方が早いのかなって気になった。
(よく他の言語でも話題になるよね)

Rustという文字を10万回、100万回結合してみる。
[Rustで文字列イテレータを連結するときに便利な itertools::join は結構遅い #ベンチマーク - Qiita](https://qiita.com/maguro_tuna/items/003a19f782884a694f8f)
とかを見ながら、
push_str使う方法、+演算子、+=演算子、format!で文字列作り直す方法、一度vectorにしてからjoinする方法の5つ。
実装の詳細はソースをみてほしい。


10万回の結果


```bash
# 10万回
push_str: 2.5595ms
+: 2.727666ms
+=: 2.480708ms
format!: 483.425208ms
join: 20.613542ms
```

最初の3つはそこまで変わらなくて、joinが次に続き、format!がかなり遅い。
なお、最初100回とかで試していたときは、format!も全然早かったけどね。


では、100万回だとどうなるか？



```bash
# 100万回
push_str: 13.588042ms
+: 14.85825ms
+=: 9.794208ms
format!: 198.9418705s
join: 217.509333ms
```

単位をよくみてほしい。
format!は、秒だ。つまり3分超えているってこと。

最初3は10msとか15msで終わっているわけだから、いかに遅いかが分かると思う。
これがやっぱりスタックに乗っているメモリなのか、ヒープなのかってことの違いかな？
でもjoinでも使っている気がするんだけどなー。
ここらへんまだ理解できていない。
下記とかも参考になるかも。
[【Rust】なぜformat!は遅いのか？ #Rust - Qiita](https://qiita.com/yonaka15/items/a44774ed06ec9f1a7951)



とりあえず、今日はそんなところ。





