---
layout: post
title: "ブログのためのJekyll環境をDockerで用意した"
tags : [環境構築, Docker, ブログ]
date: 2025-11-19 10:14:44
---


このブログは、Jekyllで用意して、Github Pagesで公開している。

[ryotakato/ryotakato.github.io](https://github.com/ryotakato/ryotakato.github.io)


最初の頃はローカルにJekyllいれてちゃんと確認していたんだけど、
慣れてきてからは、
ただ記事を書くだけならローカルで確認せずともそのままGithubにPushして公開する流れになってた。

そのうちマシンも代わったりして、随分Jekyllインストールをしてなかった。

今回ポートフォリオページを追加するにあたって、
一度ローカルで確認したいなって思ったんだけど、
わざわざそのためだけにRubyいれてJekyllいれるのは面倒だったので、
Dockerで用意することにした。

ありがたいことにJekyll公式がDockerイメージを用意してくれているので楽勝だろうって思ったら、
ちょっと古いらしく、一発では動かなかった。

ということで、下記を参考にしてDockerfileとcompose.yamlを用意した。

[DockerでJekyllのイメージを動かそうと思ったらエラーで起動しない[解決]｜sistersatori](https://note.com/sistersatori/n/nf2e6660661df)


そのままなんだけど、載せておくと、

Dockerfile

```
FROM jekyll/jekyll:latest
RUN gem install webrick
```

compose.yaml

```yaml
services:
  service_jekyll:
    container_name: local_jekyll
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - .:/srv/jekyll
    command: jekyll serve --force_polling --watch --verbose --trace
    ports:
      - "4000:4000"
```



webrickを別途インストールしないといけないみたい。


これで、
```bash
$ docker compose up
```

で起動して、localhostの4000にブラウザでアクセスすれば確認できる。

何度も修正してってやる場合は-dオプションつけたほうが楽かな。




