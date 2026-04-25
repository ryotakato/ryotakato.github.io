---
layout: post
title: "RustでCコンパイラ その11 STEP19まで(ポインター導入)"
tags : [Rust]
date: 2026-04-25 16:46:44
---


下記をRustで実装する続き。


[低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook)


Githubは下記。
commitコメントに、learnt step N って書いているから、該当のところをみれば変更が分かると思う。


[ryotakato/tvcc](https://github.com/ryotakato/tvcc)



今回はポインタ導入の、STEP18と19。
書いてあることは分かるんだけど、どう実装するのが一番よいのか分からなくて、
下記の参照実装を見た。

[Make pointer arithmetic work · rui314/chibicc@a6bc4ab](https://github.com/rui314/chibicc/commit/a6bc4ab101c20b6398fd6bbfe124665bb7db5d25)
[Add keyword "int" and make variable definition mandatory · rui314/chibicc@b4e82cf](https://github.com/rui314/chibicc/commit/b4e82cf7ce1cbfff8dd30f20fdad73fd3f1d5ccb)

ちなみに後者のコミットには、int導入も入っていて、
そこらへんはすでに実装している。

また、参照実装ではまだ存在しない関数機能も入っているので、
最終的には、下記コミットのソースぐらいまではたどり着いている。
[Support function definition up to 6 parameters · rui314/chibicc@aacc0cf](https://github.com/rui314/chibicc/commit/aacc0cfec24e0aef1e884ac8b657e182a33a7b1c)



で、ポインタの実装として難しかったのが、
ポインタそのものより、型というものの存在が明確に出てきて、それをどう持つのかってこと。
Rustは所有権があるから、型データはNodeに入れ、そこに所有権をもたせる必要がある。

しかも、加算、減算の演算子のときに、
左辺と右辺のそれぞれのNodeの型を計算しておかないといけないが、
これ以外の演算子のときはわざわざ型を算出する必要もないから、必要になったら計算する遅延評価の仕組みを考えなくてはいけなかった。

で、その遅延評価によって型を算出するメソッド(Nodeのtyメソッド)で、可変参照がいっぱい出てきて頭おかしくなりそうだった。
中々コンパイル通らないし。

結果敵には自分が参照の仕組みや、BoxやOptionを完璧には理解していなかったからで、
基本に戻ってそこをもう一度復習していったら、だんだんとできるようになっていった。

まじでRustはコンパイル通ったらほぼ期待通りに動いてくれるけど、
そこにたどり着くまでが長い。



ただ、今回のことで、ポインタに関する理解はかなり深くなった。
C言語で大きめなプログラムは書いたことないけど、
裏でどういうことをしているのかがよく分かった。

次はSTEP20以降(sizeofや配列)なんだけど、少し時間あくかもしれない。
