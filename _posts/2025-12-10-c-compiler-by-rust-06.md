---
layout: post
title: "RustでCコンパイラ その06 STEP15まで(関数の呼び出しと定義)"
tags : [Rust]
date: 2025-12-10 14:19:44
---


下記をRustで実装する続き。
[低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook)


Githubは下記。
commitコメントに、learnt step N って書いているから、該当のところをみれば変更が分かると思う。

[ryotakato/tvcc](https://github.com/ryotakato/tvcc)



STEP13で一旦ブロックを導入してから、STEP14,15なんだけど、
参照実装である、下記のコミットをみながら進めていた。

[rui314/chibicc: A small C compiler](https://github.com/rui314/chibicc)


ただし、ベースが本記事と少し違うし、
順番も少し前後しているから、読み取るのに苦労する。
例えば、本記事には空行である、;; みたいな行の処理の仕方は書いてないので、参照実装を見ながら追加していった。
後は、アセンブリの最適化がほどこしてあるから、出力したものが結構違うことがほとんど。

まあだけど、だいたい本記事の方針通りに書いていくことができたと思う。
それに、だんだん、
書いてある通りに作るんじゃなくて、
自分なりにRustで工夫して考えていくことができている。


特にSTEP14,15の関数呼び出しおよび定義なんかは顕著で、
最初は、NodeとNodeKindをわけて作っていたんだけど、
このタイミングでNode Enum一つにまとめた。
おかげで結構シンプルになったのではないかと思う。（下記コミット）

[integrate Node and NodeKind · ryotakato/tvcc@76de37d](https://github.com/ryotakato/tvcc/commit/76de37dd4860f44debc7fe43a4eefd86734e5a4b)


今回一番難しかったのは、
System V AMD64 ABI
を理解することかな。


[System V Application Binary Interface](https://refspecs.linuxbase.org/elf/x86_64-abi-0.99.pdf)


もちろん全部は理解していないけど、
関数呼び出しの引数に使えるレジスタがどれを使えばいいのかは分かった。


結構レジスタは名前が紛らわしいので、下記記事の表みたいなので全体の構成を掴むといい。

[x86-64機械語入門](https://zenn.dev/mod_poppo/articles/x86-64-machine-code)




なお、このABIの説明は本記事にも付録1を読むことで結構分かったりするんだけど、僕は後で気付いた（笑）



実際に作っていく中でぶち当たった問題としては、
アセンブリがちゃんと出力されていなくて、デバッグして原因究明などをやっているんだけど、
アセンブリのデバッグが結構面倒。
数行しかないならいいけど、フィボナッチ数列の関数再帰呼び出しとか、どこを読んでいるのか分からなかったりで、
一々 info registersや、disassを打たないといけないのが面倒だったな。

これ、ステップ実行したらdisassやregisterの値を自動で更新してくれるツールないのかな。
ってかTUIで自分で作ればいいのかな。
うーん。



次はSTEP 16以降の、ポインタと文字列に入っていく。
ポインタらへんはメモリ操作が難しそうだな。
まだ型も導入していないし。







