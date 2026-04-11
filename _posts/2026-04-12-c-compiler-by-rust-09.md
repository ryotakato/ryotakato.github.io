---
layout: post
title: "RustでCコンパイラ その09 STEP17まで(キーワードintの導入)"
tags : [Rust]
date: 2026-04-12 11:04:44
---


下記をRustで実装する続き。


[低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook)


Githubは下記。
commitコメントに、learnt step N って書いているから、該当のところをみれば変更が分かると思う。

[ryotakato/tvcc](https://github.com/ryotakato/tvcc)



STEP17は、ようやくintというキーワード。
つまりは型を導入する。

といっても、変数宣言だけなので、宣言と同時に初期化などはできない。
また、複数の変数を一度に宣言とかもできない。
ただ、これにより、下記のように関数の戻り値や引数の定義でintという型が使えるようになる。

```c
int add3(int a, int b) {
    int c;
    c = 3;
    return a + b + c;
}
```

今回は参照実装を見ずに、
自分で考えて実装してみた。

最初、自分の言語のBNFをもう覚えてなくて、いちいちプログラム読むのもだるかったので、
README.mdにBNF (正確にはEBNF) を書いておいて、
言語に機能足す度にこれを更新していくようにした。

そこからintを追加したけど、
まあ実装は少し時間くったけど、
エラーなくテストも完了。
アセンブリを生成するgeneratorはほぼいじらなかったけど、これでよいのかって不安はある。
余計な値をどっかでスタックに乗せている気がするので、このコンパイラでプログラムをコンパイルしたら、いつかメモリ枯渇するかも。


で、そのあと参照実装である下記をみたんだけど、

[Add keyword "int" and make variable definition mandatory · rui314/chibicc@b4e82cf](https://github.com/rui314/chibicc/commit/b4e82cf7ce1cbfff8dd30f20fdad73fd3f1d5ccb)


複数宣言も、宣言と同時に初期化もやっとるやないかい！
ってことで、この参照実装にはまだたどり着いていない。

だけど、参照実装は、順番がSTEPどおりになっていないので、
先に、下記でポインタ型導入していたりする。

[Make pointer arithmetic work · rui314/chibicc@a6bc4ab](https://github.com/rui314/chibicc/commit/a6bc4ab101c20b6398fd6bbfe124665bb7db5d25)

なので、あとでまとめてやるつもり。
いつか追いつけたらいいなーぐらいで。



次はSTEP18。ポインタ型。

