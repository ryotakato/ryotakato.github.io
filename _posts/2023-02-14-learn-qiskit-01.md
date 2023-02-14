---
layout: post
title: "Qiskitで始める量子プログラミング その1"
tags : [Python, 量子コンピュータ]
date: 2023-02-14 20:23:20
---


数年前から量子コンピュータが話題になっているが、
そろそろ量子プログラミングを勉強してみようかと思った。

最初は東京大学が出している、

[量子コンピューティング・ワークブック](https://utokyo-icepp.github.io/qc-workbook/welcome.html)

で学ぼうとしたのだけど、
すでに最初のほうからよく分からなかった。

Qiskitというライブラリが使われているらしく、まずそこから勉強しないといけないと思い、探すと、

[Qiskit を使った量子計算の学習](https://qiskit.org/textbook/ja/preface.html)

Qiskit本家の「Qiskitテキストブック」があったので、こっちで始めてみた。

今回は準備編。




### 環境の準備

PC自体は、Ubuntu 22.04 LTS 

で、どのようにPython環境を作ろうかなと思った。
いつもならvenvで仮想環境作るが、
Qiskitテキストブックが、Jupyter notebookで書かれているので、思い切ってJupyterでやってみることにした。
（正直Jupyterはあんまり好きじゃないんだよねー。AzureでDatabrick使ったときもそう思った。）

で、Dockerとかないかなと探すと、そのものずばりが見つかった。

[Docker, Jupyter-notebookでQiskitを使った量子計算の学習 - Qiita](https://qiita.com/gAuk/items/766dccae5c44a955883c)


これは楽でいいや。
ただ、そのままだと、下記に書いてあるオプションが有効にならず、

[Qiskitテキストブックで作業するための環境設定ガイド](https://qiskit.org/textbook/ja/ch-prerequisites/setting-the-environment.html)

Qiskitテキストブックと微妙に違うので、ちょっと改造してみた。

[ryotakato/docker-for-qiskit](https://github.com/ryotakato/docker-for-qiskit)

必要な設定ファイルをconfigディレクトリ内にあらかじめ用意しておき、
それをdocker-compose.ymlのvolumesで、該当の場所にマウントする。
こうすることで、dockerを立ち上げたときに最初からQiskitテキストブックと同じ環境となる。

ちなみに当たり前だが、Jupyterで書いたファイルはdocker内にあるので、
docker-compose upで立ち上げた後には、
docker-compose downで消してはいけない。
必ずdocker-compose stopか、ブラウザで開いているJupyterのメニューからShutdownする。




### まとめ


今は「1. 量子状態と量子ビット」の「1.2 計算の原子」が終わったところ。







