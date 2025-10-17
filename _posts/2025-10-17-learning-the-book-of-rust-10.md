---
layout: post
title: "「The Rust Programming Language」でRustを学ぶ その10 ポインタ(15章)"
tags : [Rust]
date: 2025-10-17 15:07:44
---



[The Rust Programming Language 日本語版 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/title-page.html)

の、今回は15章「スマートポインタ」

ソースは下記の、smartpointerディレクトリ
[ryotakato/rust_the_book_src](https://github.com/ryotakato/rust_the_book_src)


自分で使いこなせる気がしないけど、まあ書いてあることは分かる。
循環参照あたりの表現なんか、自分で言語処理系作ったら必須になりそうな気がしている。

で、Dropトレイト習ったところで、
自前のBox実装 MyBox にDropトレイトを実装させて値を出力させてみようとしたら、下記コードになった。

```rust
use std::ops::Deref;
use std::fmt::Display;

struct MyBox<T: Display>(T);

impl<T: Display> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T: Display> Deref for MyBox<T> {
    type Target = T;
    fn deref(&self) -> &T {
        &self.0
    }
}

impl<T: Display> Drop for MyBox<T> {
    fn drop(&mut self) {
        println!("Drop trait: {}", &self.0);
    }
}
```


うん、println!できないといけないから、 Tのトレイト境界に、Displayが入っている。
これ自体は別にいいんだけど、Deref実装のコードにも、T: Displayを書かないといけない。
Derefの中ではDisplayは必須ではないのに。
もちろんこれは試しのコードで、Dropでprintln!することなんて実際にはないのは分かるが、そういうことじゃなくて、関係ないトレイト境界を書かないといけないのかって話なんだよね。

うーんこれがなんだか気持ち悪くて、どうにかできないのかと調べたけども、 Rustではこれは仕様みたいで、どうにもできなかった。

もっとRust詳しくなったら分かるかも。
とりあえずこれは今はこのまま。


次は、並行性。苦手な系統が入ってきたな。


