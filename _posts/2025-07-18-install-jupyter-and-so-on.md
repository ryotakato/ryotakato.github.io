---
layout: post
title: "PythonのJupyter Labのインストールと環境更新もろもろ"
tags : [Mac, 環境構築, Python]
date: 2025-07-18 11:32:44
---


今までJupyterは敬遠していたのだけど、
Pythonデータサイエンスハンドブックに手を出したので、
これを機にJupyter導入してみる。
どうでもいいけど、"Jupyter"ってpyがスペルに入っているから選ばれたのかな。

<div class="AmaQuick-box" style="margin-bottom: 0px;"><div class="AmaQuick-image" style="float: left; margin: 0px 12px 1px 0px;"><a href="https://www.amazon.co.jp/dp/4814400632/?tag=tavi06-22" name="AmaQuicklink" target="_blank"><img src="https://m.media-amazon.com/images/I/4104emmrmWL._SL200_.jpg" alt="Pythonデータサイエンスハンドブック 第2版 ―Jupyter、NumPy、pandas、Matplotlib、scikit-learnを使ったデータ分析、機械学習" style="border: none;"/></a></div><div class="AmaQuick-info" style="margin-bottom: 10px; line-height: 120%"><div class="AmaQuick-name" style="margin-bottom: 10px; line-height: 120%"><a href="https://www.amazon.co.jp/dp/4814400632/?tag=tavi06-22" name="AmaQuicklink" target="_blank">Pythonデータサイエンスハンドブック 第2版 ―Jupyter、NumPy、pandas、Matplotlib、scikit-learnを使ったデータ分析、機械学習</a><div class="AmaQuick-powered-date" style="font-size: 80%; margin-top: 5px; line-height: 120%">posted with <a href="https://creazy.net/amazon_quick_affiliate" title="AmaQuick" target="_blank">AmaQuick</a></div></div><div class="AmaQuick-detail">Jake VanderPlas(著), 菊池 彰(翻訳)</div><div class="AmaQuick-sub-info" style="float: left;"><div class="AmaQuick-link" style="margin-top: 5px"><a href="https://www.amazon.co.jp/dp/4814400632/?tag=tavi06-22" name="AmaQuicklink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="AmaQuick-footer" style="clear: left"></div></div>

Jupyterは今はJupyter Notebookではなく、
Jupyter Labってのが主流らしく、こっちをインストールする。

まず、概念として、
JupyterはPythonの実行をグラフィカルに表示できるようにブラウザ上でできるようにしたもの。
ただし、Pythonコード自体は裏で動いているから、裏のPythonバージョンは自分のマシンにインストールしたやつを選べる。

で、IPythonとの兼ね合いがややこしいけど、下記が分かりやすい。

[【図解】IPythonとJupyterの関係を簡単に図解してみた #ipykernel - Qiita](https://qiita.com/shoheihagiwara/items/f0d426cc8a2cec999107)

昔IronPythonってのがあったなと思って、
それとIPythonを同じものと思っていたけど、別物だったんだよね。
IはInterfaceのIかな。

まあそれは置いといて、
インストールに進みたいけど、
問題はJupyterをどこにいれるかということ。
Macだとbrewでいれる方法だったり、 pip install jupyterみたいにPythonのライブラリとしていれる方法もあり、
さらには、Dockerに入れて隔離するって手もある。

うーん、色々あって迷う。
Jupyterを色々バージョン違いで試すってことはないので、pipで入れなくてもいい気はするが、
だけど、brewでいれるほどマシンに依存させたくはないな。
かと言って、Dockerで隔離するのもメリットがないな。

迷った結果、
asdfで入れているPythonにグローバルでいれることにした。
Pythonのバージョン違いを入れれば、それらのカーネルをJupyterでも使えるし。

参考になったのは下記
[【Python】Jupyter Notebookのインストールと環境構築 - ヴェズルフェルニルの研究ノート](https://blog.ketus-ix.work/entry/2023/12/21/091108)



で、インストールを進めるにあたって、最近色々更新していなかったので、更新しておく。




### 環境情報

まずは環境情報。MacBookAir の、2023 M2 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		15.3.1
BuildVersion:		24D70
```


### Homebrew更新

```bash
$ brew update

$ brew --version
Homebrew 4.5.10
```

### asdf更新


```bash
$ brew upgrade asdf

$ asdf version
0.18.0 (revision unknown)
```

asdfは0.16.0からコマンド体系が変わったらしく、globalやlist-allがなくなっていた。
[0.16.0にアップグレードする &#124; asdf](https://asdf-vm.com/ja-jp/guide/upgrading-to-v0-16)


### 最新版のPythonインストール

```bash
# インストール
$ asdf install python 3.13.5

# バージョン設定
$ asdf set -u python 3.13.5

# reshim
$ asdf reshim
```

Pythonの3.13から、3.13.5tっていう、tがつくバージョンがあって、
これはなんだろうと調べてみたら、
Free Threadingという機能が追加されていて、
今までPythonにあったGILというスレッドの同時実行を制御する仕組みを改善して、
本当のマルチプロセスを実現しているらしい。
っだが、一旦この機能は今はそこまで必要としていないので、普通のをいれた。
numpyとかで速度が問題になったら入れてみよう。



### Jupyter Labインストールと起動


```bash
# インストール
$ pip install jupyterlab
# 起動
$ jupyter lab
```


初期設定は、テーマを変えたり色々。





