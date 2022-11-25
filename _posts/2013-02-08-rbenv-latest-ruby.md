---
layout: post
title: "rbenv で ruby の最新を使う"
tags : [Ruby, 環境構築]
date: 2013-02-08 07:26:30
---



**(2013/11/06追記) 下記の内容はすでに古いです。**


久々にRubyの最新を入れようと思って、ちょっと困ったので、備忘録として。


rbenvとruby-buildをgithubから直接入れていたので、下記手順が必要


1. rbenvをgit pull
2. ruby-buildをgit pull
3. ruby-buildのinstall.shを実行


1は別に必須ではないが、気持ちが悪いので併せてアップデート
2が終わった段階でrbenv install -l とかやっても、最新版のRubyは表示されないので、
3を忘れずに。


