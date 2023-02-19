---
layout: post
title: "Qiskitで始める量子プログラミング その2"
tags : [Python, 量子コンピュータ]
date: 2023-02-19 12:12:20
---

今回は、「1.3 量子ビット状態を表現する」

[量子ビット状態を表現する](https://qiskit.org/textbook/ja/ch-states/representing-qubit-states.html)



ここらへんから結構難しくなってくる。
事前に、線形代数入門のページをみておけって話があったので、
目を通したが、なんとなく分かるという部分ばかり。

[線形代数入門](https://qiskit.org/textbook/ja/ch-appendix/linear_algebra.html)


とりあえず、そういう理解でいいので、このページの「スパン集合、線形従属、そして基底」まで読んでおけば、
今回のページは読み進めることができた。

特に難しかったのが、グローバルフェーズのところと、
最後の練習問題。

練習問題は、
「制限された量子ビット状態の説明」に記載のある三角関数の入った式の形にしてやれば、
θとΦが求められるのだが、ネイピア数eの累乗がどうすればいいのか分からなくて悩んでた。

で、調べてみたら、オイラーの公式があったことに気づいた。

[オイラーの公式 &#124; 三角関数・複素指数関数・虚数が等式として集約されるまでの物語 - 空間情報クラブ &#124; インフォマティクス運営のメディアサイト](https://club.informatix.co.jp/?p=3060)


ここのxに虚数iを代入したら-1になることを利用して、
Φが、πとかπ/2とかに求められる。

これに気づいたとき、オイラーすげーって思ったね。なんか馬鹿みたいな感想だけど。


答え合わせは有志が作ったと思われる下記ページで。

[wg-quantum/qiskit_textbook_answers](https://github.com/wg-quantum/qiskit_textbook_answers)



成果物は、下記のstudiedディレクトリにいれている。


[ryotakato/docker-for-qiskit](https://github.com/ryotakato/docker-for-qiskit)


あと、Jupyter Notobook上で数式表示するためにTeXについて軽く学んだ。
ディラック記法で書きたかったので、下記のハックを利用した。
[Qiitaでディラック記法を綺麗に表示する方法 - Qiita](https://qiita.com/yyu/items/c140fbbd1236fe25cc7a)



とりあえず今日はここまで。
まだ量子コンピュータ感が全然ない。







