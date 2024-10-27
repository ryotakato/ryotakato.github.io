---
layout: post
title: "「作って動かすALife」で人工生命を学ぶ その01 Gray-Scottモデル"
tags : [Python, 人工生命]
date: 2024-10-29 06:52:44
---



クライストチャーチの図書館ではE-Bookの閲覧も結構豊富なので、
興味がある人工生命分野の何かの本を読んでみようと思った。

<div class="AmaQuick-box" style="margin-bottom: 0px;"><div class="AmaQuick-image" style="float: left; margin: 0px 12px 1px 0px;"><a href="https://www.amazon.co.jp/dp/4873118476/?tag=tavi06-22" name="AmaQuicklink" target="_blank"><img src="https://m.media-amazon.com/images/I/516dQmDuT3L._SL200_.jpg" alt="作って動かすALife ―実装を通した人工生命モデル理論入門" style="border: none;"/></a></div><div class="AmaQuick-info" style="margin-bottom: 10px; line-height: 120%"><div class="AmaQuick-name" style="margin-bottom: 10px; line-height: 120%"><a href="https://www.amazon.co.jp/dp/4873118476/?tag=tavi06-22" name="AmaQuicklink" target="_blank">作って動かすALife ―実装を通した人工生命モデル理論入門</a><div class="AmaQuick-powered-date" style="font-size: 80%; margin-top: 5px; line-height: 120%">posted with <a href="https://creazy.net/amazon_quick_affiliate" title="AmaQuick" target="_blank">AmaQuick</a></div></div><div class="AmaQuick-detail">岡 瑞起(著), 池上 高志(著), ドミニク・チェン(著), 青木 竜太(著), 丸山 典宏(著)</div><div class="AmaQuick-sub-info" style="float: left;"><div class="AmaQuick-link" style="margin-top: 5px"><a href="https://www.amazon.co.jp/dp/4873118476/?tag=tavi06-22" name="AmaQuicklink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="AmaQuick-footer" style="clear: left"></div></div>

この本は人工生命の様々なアプローチを一冊にまとめて、しかもPythonで実装できるというので手を出してみた。



まえがきや1章を読んで、現在2章。
今の環境だと動かないところもあったので、書き残しておく。



まずは環境情報。


MacBookAir の、2023 M2 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		14.6.1
BuildVersion:		23G93
```

そして準備

```bash
$ mkdir alife_oreilly
$ cd alife_oreilly
$ python -m venv .venv
$ source .venv/bin/activate
$ pip install --upgrade pip
```

まえがきで触れてあったように、各ライブラリを入れる。


```bash
$ pip install numpy
$ pip install vispy
$ pip install PyQt5
```

PyQtは、今はPyQt6も出ているのだが、本に合わせて5にする。



で、ここまで入れて、普通に2章のGray-Scottモデルを進め、
いざ実行しようとしたら、下記エラーが出た。

```bash
$ python gray_scott.py 
Traceback (most recent call last):
  File "/Users/tavi/develop/python/alife_oreilly/src/chap02_gray_scott/gray_scott.py", line 4, in <module>
    from alifebook_lib.visualizers import MatrixVisualizer
  File "/Users/tavi/develop/python/alife_oreilly/src/chap02_gray_scott/../alifebook_lib/__init__.py", line 1, in <module>
    import vispy
  File "/Users/tavi/develop/python/alife_oreilly/src/.venv/lib/python3.12/site-packages/vispy/__init__.py", line 27, in <module>
    from .util import config, set_log_level, keys, sys_info  # noqa
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/tavi/develop/python/alife_oreilly/src/.venv/lib/python3.12/site-packages/vispy/util/__init__.py", line 14, in <module>
    from . import fonts       # noqa
    ^^^^^^^^^^^^^^^^^^^
  File "/Users/tavi/develop/python/alife_oreilly/src/.venv/lib/python3.12/site-packages/vispy/util/fonts/__init__.py", line 13, in <module>
    from ._triage import _load_glyph, list_fonts  # noqa, analysis:ignore
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/tavi/develop/python/alife_oreilly/src/.venv/lib/python3.12/site-packages/vispy/util/fonts/_triage.py", line 14, in <module>
    from ._quartz import _load_glyph, _list_fonts
  File "/Users/tavi/develop/python/alife_oreilly/src/.venv/lib/python3.12/site-packages/vispy/util/fonts/_quartz.py", line 12, in <module>
    from ...ext.cocoapy import cf, ct, quartz, CFRange, CFSTR, CGGlyph, UniChar, \
  File "/Users/tavi/develop/python/alife_oreilly/src/.venv/lib/python3.12/site-packages/vispy/ext/cocoapy.py", line 1288, in <module>
    quartz = cdll.LoadLibrary(util.find_library('quartz'))
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/tavi/.asdf/installs/python/3.12.5/lib/python3.12/ctypes/__init__.py", line 460, in LoadLibrary
    return self._dlltype(name)
           ^^^^^^^^^^^^^^^^^^^
  File "/Users/tavi/.asdf/installs/python/3.12.5/lib/python3.12/ctypes/__init__.py", line 379, in __init__
    self._handle = _dlopen(self._name, mode)
                   ^^^^^^^^^^^^^^^^^^^^^^^^^
OSError: dlopen(Quartz.framework/Quartz, 0x0006): tried: 'Quartz.framework/Quartz' (no such file), '/System/Volumes/Preboot/Cryptexes/OSQuartz.framework/Quartz' (no such file), '/Users/tavi/.asdf/installs
/python/3.12.5/lib/Quartz.framework/Quartz' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/Users/tavi/.asdf/installs/python/3.12.5/lib/Quartz.framework/Quartz' (no such file), '/opt/homebrew/lib/Qu
artz.framework/Quartz' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/opt/homebrew/lib/Quartz.framework/Quartz' (no such file), '/Users/tavi/.asdf/installs/python/3.12.5/lib/Quartz.framework/Quartz
' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/Users/tavi/.asdf/installs/python/3.12.5/lib/Quartz.framework/Quartz' (no such file), '/opt/homebrew/lib/Quartz.framework/Quartz' (no such file), '/S
ystem/Volumes/Preboot/Cryptexes/OS/opt/homebrew/lib/Quartz.framework/Quartz' (no such file), '/usr/lib/Quartz.framework/Quartz' (no such file, not in dyld cache), 'Quartz.framework/Quartz' (no such file)
```

Googleさんに聞いてみると、
どうやらvispyで今年5月あたりに修正が入ったみたい。
だけど、その時期の修正ならもうすでに本体に取り込まれているはずだからおかしいなと思って、
もっと調べてみると、

[Fix MacOs 14 Sonoma dlopen cache lookup by takacsmark · Pull Request #2549 · vispy/vispy](https://github.com/vispy/vispy/pull/2549)

このIssueの最後に、情報があったので、

```bash
$ pip install pyopengl
```

とやってみると、 
起動はしたのだが、何も表示されない。

調べると、

[gray_scott.pyが動かない · Issue #29 · alifelab/alife_book_src](https://github.com/alifelab/alife_book_src/issues/29)

の最後に、
matrix_visualizer.pyの__init__関数の最後を書き換えると良いとあったので、
それを実際にやってみると、

![learning_oreilly_alife_01_01]({{ BASE_PATH }}/images/2024/10/29/learning_oreilly_alife_01_01.png)


動いた！

まだ初期化しかしていないので、何も動かないけど、とりあえず最初の1歩。



その後、ラプラシアン計算やGray-Scott方程式をプログラムに組み込んでいくと、


Stripe柄

![learning_oreilly_alife_01_02]({{ BASE_PATH }}/images/2024/10/29/learning_oreilly_alife_01_02.png)


Spot柄

![learning_oreilly_alife_01_03]({{ BASE_PATH }}/images/2024/10/29/learning_oreilly_alife_01_03.png)


などの模様を作ることができた。
ラプラシアン計算などは難しいけど、なんとか理解できたし、
Numpyの使い方とかも学べた(np.rollとか)

次は同じ2章だけど、セルオートマトンで、ライフゲーム。
これは馴染みある話題なので、結構簡単にできそう。


