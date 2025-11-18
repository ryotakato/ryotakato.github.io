---
layout: post
title: "RustでCコンパイラ その03 STEP7まで(電卓レベルの言語の作成が完了)"
tags : [Rust]
date: 2025-11-18 13:10:44
---


下記をRustで実装する続き。
[低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook)


Githubは下記。
commitコメントに、learnt step N って書いているから、該当のところをみれば変更が分かると思う。

[ryotakato/tvcc](https://github.com/ryotakato/tvcc)



STEP3からは、大幅に改造して、モジュールわけも進めた。

再帰下降構文解析 (Recursive Descent Parsing) のために、抽象構文木 (AST = Abstract Syntax Tree) を作る必要があるので、
これもTokeniserと同じくLinkedListで表現した。
だけど、左辺と右辺を持つので2つの値をそれぞれ、Option<Box<Node>> で持つ。

Tokeniserで苦労して理解した分、そこまで難しくは感じなかったけど、
元記事で書いてある、consumeやexpect関数内で現在指している先のポインタの更新を、どうRustで表現するかがちょっと悩んだかな。
Tokeniserでトークン化したTokenListは自前のIteratorを実装していたんだけど、
Iteratorってnextしかないから、値を消費せずに中身をチェックする方法がない。
なので、今回は該当のIteratorにcurrentメソッドを追加して、次のnextで取得できるものを参照できるようにした。(tokeniser.rs参照)
これ、RustのPeekableというものを使えば同じことできるみたいだけど、 (iter().peekable()で取得)
[Peekable in std::iter - Rust](https://doc.rust-lang.org/std/iter/struct.Peekable.html)
名前が分かりづらいのと、今回はただ参照したいだけだったから手前味噌で作った。
結果として、nextメソッドは値をもらうことはなくて、ただ次に進めるだけの使い方になってしまったのが何か綺麗じゃない気がする。
だから、いちいち let _ = で受け取っているし。 (parser.rsの、Parser構造体参照)

そんな感じでSTEP4〜STEP6でかなり試行錯誤したおかげで、STEP7のASTのところはすんなり比較演算子を追加できた。
しかし、その分、今までほっておいたTokeniserが、文字列を1文字ずつただ繰り返せばいいわけじゃなくなったので、そこを修正。
自前でToken解析を書いている。
Rustには、strtolがないから、数値を先読みできる機能を自前で書くしかないんだよな。
正規表現クレート使えばいいのだろうけど、今は勉強のためになるべく他クレートに依存したくない。

まだ納得いっていないのは構文解析などでエラーが出たときのエラー関数まわり。
cc_utilモジュールに書いているけど、これなんとかしたい。

モジュールわけしてから後で気付いたけど、次のSTEP8がまさにファイル分割してって感じなので、
もうやっていて、やることなさそう。
そのうち、このモジュールわけで不便が出てきたら改善するかも。


さらに次のSTEP9からは変数とか入ってきて、ようやく言語っぽくなる。
もっと楽しくなりそう。
やっぱり少しづつできてくるのが実感できるのはいいなー。

参考
[[Rust] Option メソッド まとめ #Rust - Qiita](https://qiita.com/kerupani129/items/a45c614279e7fc58f129)
[【Rust】モジュールとファイル分割 #Rust - Qiita](https://qiita.com/k-yaina60/items/388fa6ef30070af4844f)
