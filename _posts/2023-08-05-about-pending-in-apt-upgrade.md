---
layout: post
title: "apt upgradeで保留が出る件について"
tags : [Linux]
date: 2023-08-05 16:40:44
---

最近apt updateしてなかったなと思い、
久々にやってみると、結構更新対象が出てた。
じゃあいれるかと、upgradeをするんだけど、
Ubuntu Proの宣伝文言だけで、全然パッケージがアップデートされないの。


なんだろうと思って、apt install でパッケージ名指定すると、普通に入るんだよね。
どうやら、保留扱いになっていたから入らなかったのだろうけど、
その理由があまり載ってなかった。

で、ようやく探した情報がこれ。

[APT で upgrade/dist-upgrade できなかったとき &#124; text.Baldanders.info](https://text.baldanders.info/remark/2022/09/cannot-upgrade-with-apt/)

要するに、ほっておけばそのうちupgradeで入るようになるってことらしい。
まあじゃあ今後はあまり気にすることないかな。


ちなみにUbuntu Proは、個人だと5台まで無料らしいから入っておいてもいいかもだけど、
そもそも10年も同じLTSを使い続けることがないんだよね。
きっとその間に次のLTSに移行しているから、入るメリットがあまりない。
ってことで、今は特にProへは入らない。



