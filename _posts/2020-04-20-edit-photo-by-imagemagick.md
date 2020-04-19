---
layout: post
title: "ImageMagickを使って、写真の加工"
tags : [写真, ImageMagick]
date: 2020-04-20 05:59:44
---

備忘も兼ねて。  

撮った写真などをブログにあげる場合、  
最近のスマホのカメラだと容量が大きいので、  
写真を縮小することでファイルサイズを小さくしてアップロードしたい。  
特に写真が高画質である必要がない場合。  

また、どうせならEXIF情報も削除しておきたい。  



```bash
# 元画像の20%に削減する場合
$ mogrify -strip -geometry 20% *.JPG

# 一つのファイルだけなら、convert aaa.jpgを読み込んで同名ファイルで上書き
$ convert -strip -geometry 20% aaa.jpg aaa.jpg
```


なお、このやり方だとたまに縦横が回転してしまうことがあるが、なんでだろう。  


