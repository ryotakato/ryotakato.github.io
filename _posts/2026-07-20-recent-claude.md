---
layout: post
title: "最近のClaude Code環境"
tags : [環境構築, AI]
date: 2026-07-20 18:18:44
---



備忘録
最近使っているClaude Code関連のツールたち。


### agmsg

[fujibee/agmsg: Cross-vendor messaging for CLI AI coding agents — let Claude Code, Codex, Gemini & Copilot talk to each other in one team. Bash + SQLite, no daemon, no framework.](https://github.com/fujibee/agmsg)


複数のエージェントの会話用ツール。
sqliteを使ってローカルでの受け渡しなので、軽いというのを売りにしているらしい。
cursorと使ってみたくて何度か試しているけど、
Claude Codeでできる自動monitorが、cursorではできなくて、使うのを滞りがち。


インストールは、入れた時点でREADMEに書いてあったnpxでインストールする方法


```bash
$ npx agmsg
```


### compact-plus

[u-ichi/compact-plus: Claude Code plugin: preserve and restore working state around /compact](https://github.com/u-ichi/compact-plus)


compactをもっと賢くするツール。
下記で紹介されている。

https://x.com/u1/status/2073742408555343968?s=20



Releaseから、zipおとしてきて展開して、
その場所を、下記コマンドの /path/to/compact-plus に指定して実行でインストール可能。


```bash
$ claude plugin marketplace add /path/to/compact-plus --scope user

$ claude plugin install compact-plus@compact-plus-local
```

.claude/settings.json
には、下記だけ指定している。codexはまだ使ってないので。

```
  "env": {
    "COMPACT_PLUS_FALLBACK_BACKEND": ""
  }
```




### MulmoClaude

ブラウザでClaude Codeを立ち上げて、簡単にツールとか作ってくれるものらしい。
Claude CodeはCLIなので、描画が弱いが、そこを補うためにブラウザの描画機能を使っているのかな。

インストールはnpxで一発。
なお、Docker Desktop入っているから、Docker環境で動くようにしている。

```bash
npx mulmoclaude@latest
```

どれだけClaude Codeの使用量を使うのかなどは分かっていないので、使いながら注意していこうと思う。





