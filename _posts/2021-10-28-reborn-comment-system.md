---
layout: post
title: "ブログにコメント機能を復活させた(jekyll-comments-google-forms使用)"
tags : [ブログ]
date: 2021-10-28 00:40:44
---


一年前くらい、使ってなかったコメント機能を削除した。

[死んでたコメント機能を削除した](/2020/11/29/remove-comment-system)



このときに、コメントはツイッターでいいかと思ったんだけど、
やっぱり気軽にコメントしたいときに、SNSは気が重い。

なので、Jekyllでどうにかコメントをつけれないかと色々と探していたところ、
Google Formsなどを利用して実現している人がいた。

[Using Google Forms for Jekyll Comments &#124; JD Porterfield &#124; Articles](https://jdvp.me/articles/Google-Forms-Jekyll-Comments)
[Using Google Forms for Jekyll Comments, Revisited &#124; JD Porterfield &#124; Articles](https://jdvp.me/articles/Google-Forms-Jekyll-Comments-Revisited)

[jdvp/jekyll-comments-google-forms](https://github.com/jdvp/jekyll-comments-google-forms)

英語だけど、結構丁寧に説明してあるので、ほとんど迷わなくてすむ。
最後のはGithub


こりゃあいいと思って、早速とりこんだ。
この記事の下にもコメント欄ができているはず。



手軽さで言えば、下記のように utterances という選択肢もあったんだけど、
これだとコメントするのにGithubアカウントが必要なので、そこがちょっとなーって感じだった。

[Jekyllブログにコメント機能 - utterancesのサービスを使ってJekyllのプロジェクトへコメント機能を追加してみましょう。](https://dev-yakuza.posstree.com/jekyll/utterances/)




デザインや、reCAPTCHAなど、少しいじりたいところがあるので、
もうちょっと試行錯誤してみる。

一通りできたら、改めて記事にしてみる予定。


