---
layout: post
title: "「ソフトウェア設計」をC++で学ぶ その01 ガイドライン15"
tags : [C++]
date: 2025-01-11 23:42:44
---

[前回](/2025/01/06/begin-cpp) 、C++を始めてみたが、

学ぶついでに、ソフトウェア設計も復習したいなと思って、

<div class="AmaQuick-box" style="margin-bottom: 0px;"><div class="AmaQuick-image" style="float: left; margin: 0px 12px 1px 0px;"><a href="https://www.amazon.co.jp/dp/4814400454/?tag=tavi06-22" name="AmaQuicklink" target="_blank"><img src="https://m.media-amazon.com/images/I/41Vl7xZah3L._SL200_.jpg" alt="C++ソフトウェア設計 ―高品質設計の原則とデザインパターン" style="border: none;"/></a></div><div class="AmaQuick-info" style="margin-bottom: 10px; line-height: 120%"><div class="AmaQuick-name" style="margin-bottom: 10px; line-height: 120%"><a href="https://www.amazon.co.jp/dp/4814400454/?tag=tavi06-22" name="AmaQuicklink" target="_blank">C++ソフトウェア設計 ―高品質設計の原則とデザインパターン</a><div class="AmaQuick-powered-date" style="font-size: 80%; margin-top: 5px; line-height: 120%">posted with <a href="https://creazy.net/amazon_quick_affiliate" title="AmaQuick" target="_blank">AmaQuick</a></div></div><div class="AmaQuick-detail">Klaus Iglberger(著), 千住 治郎(翻訳)</div><div class="AmaQuick-sub-info" style="float: left;"><div class="AmaQuick-link" style="margin-top: 5px"><a href="https://www.amazon.co.jp/dp/4814400454/?tag=tavi06-22" name="AmaQuicklink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="AmaQuick-footer" style="clear: left"></div></div>



を読んでいる。
現在4章のVisitorパターンのところなんだけど、
かなりわかりやすくて、この本はおすすめ。
何故再コンパイルが必要なぐらい結合度が高い依存がダメなのかなどがしっかり説明されている。
Javaでこの世界に入った身としては、再コンパイルしないといけないぐらい許容範囲だろうと思っていたのだが、甘かったようだ。


で、実際に動かしながらなので、下記にソースを書いている。


[ryotakato/cpp_software_design](https://github.com/ryotakato/cpp_software_design)



ソースの内容自体にはそこまで苦労していないのだけど、
ガイドラインごとにディレクトリ分けして、
かつそれをコンパイルするのに、C++のインクルードとモジュール機構の概念を理解する必要があった。
本当は全部モジュールにしてしまえば良かったのだけど、
なるべく本のソースに則った形にしたかったので、
両立させるように工夫した。
あと、名前空間も設定して、ガイドラインごとに同じ名前のソースがあっても被らないようにした。

まだ、何がC++のプロジェクト構成として一般的なのかは分からないのだが、
自分で良いと思う構成を考えて進めている。

でもあれだね、C++は歴史があって、かつ色々と新しい概念を取り入れているせいで、
調べても古い情報が多く、正解を見つけるのが難しい。


