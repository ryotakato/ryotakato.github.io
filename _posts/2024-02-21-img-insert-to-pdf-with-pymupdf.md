---
layout: post
title: "PythonでPDFに画像の挿入（PyMuPDF使用）"
tags : [Python]
date: 2024-02-21 17:03:44
---


とある申し込みで、PDFに署名を入れないといけなかった。
ただし、最終的にはまたPDFで送る必要があるのだが、
PDFを印刷して署名して、スキャンで再度PCに取り込むと、
他の部分がかなり薄くなったりして、非常に見づらい。

なので、発想を変えて、
スキャンしたPDFの署名部分だけをオリジナルのファイルの所定の位置に統合できないかと思ったわけだ。

よし、こういうちょっとしたプログラムはPythonだ。
ということで、何かいいライブラリはないかと探していたら、
最初は、PyPDF2とReportLabを使った方法が見つかった。

[【Python】既存の PDF に画像を挿入する（PyPDF2 / ReportLab） - 7839](https://serip39.hatenablog.com/entry/2021/01/18/170000)



ほうほう。
それなら、とりあえずPDFの統合だけやってみたところ、
確かにPyPDF2で統合はできたのだが、
任意の位置に統合するということができないので、
どちらのPDFも端から始まってしまう。
色々試行錯誤したが、どうにもできないので、この方式は諦めた。

ただ、上記記事の中のように、
一度画像にして、それをオリジナルのPDFに差し込めばいいのかって思ったところ、


[get_pixmap - PyMuPDF 1.23.24 ドキュメント](https://pymupdf.readthedocs.io/ja/latest/page.html#Page.get_pixmap)
[insert_image - PyMuPDF 1.23.25 ドキュメント](https://pymupdf.readthedocs.io/ja/latest/page.html#Page.insert_image)

PyMuPDFというライブラリがどちらの機能もありそう。
しかもRectという考えで範囲指定もできそう。


ということで実際に作って上手くいったので、ここにソースを貼っておく。
全然汎用化できてないので、Githubにあげるには躊躇われたｗ




まずはPDFの一部を画像にして取り出すところ。
orig_sign.pdfは署名が入ったPDF。
今回は1ページしか入ってないので、1つのpngができる。
dpiを指定したのは高解像度で貼り付けたかったから。

```python
import sys, fitz
fname = "input/orig_sign.pdf"
doc = fitz.open(fname)
for page in doc:
    # 範囲指定
    crop_length = 100
    rect = fitz.Rect(page.rect[0]+95,
                     page.rect[1]+584,
                     page.rect[2]-240,
                     page.rect[3]-216)
    # トリミング
    page.set_cropbox(rect)
    pix = page.get_pixmap(dpi=300)
    pix.save("output/high_page-%i.png" % page.number)
```



次に画像を別のPDFに差し込むところ
origin.pdfは4ページあって、その4ページ目の途中に差し込みたかった。
Rectの指定は微妙なズレの調整の結果

```python
import fitz

doc = fitz.open("input/origin.pdf")

for page_index in range(len(doc)):
    page = doc[page_index]

    if page_index == 3:
        rect = fitz.Rect(page.rect[0]+120,
                     page.rect[1]+590,
                     page.rect[2]-270,
                     page.rect[3]-230)
        page.insert_image(rect,filename="output/high_page-0.png")

doc.save("output/merged.pdf")
```

今後使うことがあるのかどうかよくわからないが、
期待する結果ができて良かったー。





