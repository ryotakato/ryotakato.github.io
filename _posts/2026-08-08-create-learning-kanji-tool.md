---
layout: post
title: "漢検のための漢字練習（管理？）アプリをつくった"
tags : [つくったもの, 子供, JavaScript, AI]
date: 2026-08-08 15:27:44
---


以前、 
[ポケモンのパーティ管理アプリをつくった](/2026/06/11/create-pokemon-party-tool)
を書いたが、
同じSvelteKit構成で、漢字学習アプリを作った。


[漢字学習アプリ](https://blog.tavi-travelog.net/learning-kanji/)



下記GithubのPagesで公開している。

[ryotakato/learning-kanji: A learning application for kanji](https://github.com/ryotakato/learning-kanji)


同じ構成なので、あまり語ることはないが、
漢字データおよび、問題を出すための熟語や単語データを集めるのが大変だった。
クレジットにどこからとってきたかは書いてあるが、
そもそもそれだけでは学年に合わせた良い問題にならないので、
結構自分の目で見直したり、
Claude Codeに基準を与えてスクリーニングさせたりした。
Opus5がでてから、結構判断がうまくなっている気がする。


子供が漢検を受けているので、その助けになればと思い。
使い方などはまだ書いてないので、これから改善していく予定。
特に、A,B,C,Dとか意味が分からないしね。
ここは僕と子供のやり方を元にしているから、いつか漢字の勉強方法として記載予定。















