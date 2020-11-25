---
layout: post
title: "ffmpegを使って、動画の加工"
tags : [動画, ffmpeg]
date: 2020-11-25 20:29:44
---

備忘も兼ねて。  

以前、下記記事を書いたが、
[ImageMagickを使って、写真の加工](/2020/04/20/edit-photo-by-imagemagick)
今度は動画。

iPhoneで撮った動画はmov形式なので、
ブログに載せるときはmp4とかにしないと、
見れないブラウザがあるかもしれない。

そのため、Linuxで気軽に変換できるffmpegを使ってみた。


```bash
# インストール
$ sudo apt install ffmpeg

# mov形式をmp4形式に変換しながら、90度反時計回りに回転する。
# metadataのrotateで元の回転情報を削除
$ ffmpeg -i aaa.MOV -vf "transpose=2" -metadata:s:v:0 rotate=0 aaa.mp4

```

transposeオプションは、下記
1：90度時計回りに回転
2：90度反時計回りに回転
3：90度時計回り回転後、上下を反転
0：90度反時計回り回転後、上下を反転

サイズとしては、
mov形式をmp4形式に変換したら、だいたい40%ぐらいになった。

試したのは、6秒ぐらいの変換だけど、20秒ぐらいかかったかな。

