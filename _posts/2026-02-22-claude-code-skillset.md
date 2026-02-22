---
layout: post
title: "Claude CodeのSkillsetを作った"
tags : [環境構築, AI]
date: 2026-02-22 16:26:44
---


Claude Codeの使い方として、下記を読んだところ、かなり自分にもしっくりきた。

[How I Use Claude Code &#124; Boris Tane](https://boristane.com/blog/how-i-use-claude-code/)



そこで、このSTEPをスキル化してみた。


[ryotakato/claude_code_skillset](https://github.com/ryotakato/claude_code_skillset)





### 現状のスキルセット

* 01-research: リサーチ、分析用
* 02-planning: 計画策定用
* 03-annotated: 計画見直し用
* 04-implementation: 実装用
* 05-feedback: レビュー用

まあ開発者用だね。



別にプロンプトで指示してもいいんだけど、
スキル化の何がいいかというと、
「現状分析して」 とか、「注釈つけたから調査結果を見直して」とかいうだけで、
ディレクトリの場所とかも指示した通りになっているし、
各工程のmarkdownファイルをちゃんと更新してくれる。

もちろん、タスクの内容の細かい指示はしないといけないが、
ファイルに保存してもらうとかの本質的でないところを気にしなくて済むのが最高。



前回の [Claude Codeと遺伝的アルゴリズムでねづっち](/2026/02/17/claude-code-play-on-words-by-ga) を少しこのフローで修正しているが、
ちゃんとフローが流れていくし、何の変更をしたのかをちゃんと、ドキュメントとして残してくれるので、非常に助かった。

ようやく自分の中でClaude Codeでの開発の流れが固まってきた感じ。

