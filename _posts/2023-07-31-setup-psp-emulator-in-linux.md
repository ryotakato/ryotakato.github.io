---
layout: post
title: "LinuxでPSPエミュレータの準備"
tags : [Linux, ゲーム]
date: 2023-07-31 19:33:44
---

PSP-3000を持っているが、
いつ使えなくなるかわからないので、PCでできるようにエミュレータに移行しておく。





### PPSSPPのインストール

PSPのエミュレータといえば、PPSSPP一択の状況らしい。


```bash
# PPAを追加
$ sudo add-apt-repository ppa:xuzhen666/ppsspp
$ sudo apt update
$ sudo apt install ppsspp

```

これで入るバージョンは、2023/07/31時点の最新版である、1.15.4 だった

参考にしたのはここ
[Linux Mint 20.x : PSP エミュ「PPSSPP」のインストール方法 | 221B Baker Street](http://baker-street.jugem.jp/?eid=134)





### PSPのソフトを吸い出し

何個か工程があるので、参考にしたのはここ

[PSPのゲームを吸い出してエミュレータで動かすまで。完全マニュアル｜GameCenter WASABI](https://note.com/arcade/n/n7e166c1cd5bc)





まずはPSPのバージョンを6.61にする。
6.60だったからWifiアクセスして、バージョンアップを走らせるだけで良かった。

次にCFWを入れる
SDカードは、元々PSPで使っていた4GBのやつを使った。
中身は全部コピーしてPCにバックアップしておく。

でFATでフォーマットし、PSPでもフォーマットする。


次に、infinity-2.0.3と661_PRO-C2_14-02-2015bを下記からダウンロード。

[Infinity](https://infinity.lolhax.org/)

[Free large file hosting. Send big files the easy way!](https://www.sendspace.com/file/o0snfm)



SDカードに、PSP/GAME/UPDATEディレクトリを作って、
infinityのstandardの中の、EBOOT.PBPをコピーし、
661_PRO-C2_14-02-2015bのPSPディレクトリの中身をSDカードの同じ場所にコピーしておく。

PSPにて、
CFW660PRO Update起動からのPROのインストール、
Infinity2起動からのインストールをしたら、
Infinity2を再起動して、PRO CFW by Coldbirdを選択して、startボタンから再起動しておく。
再度CFW660PRO Update起動

これでCFWがインストールされているはずなので、
本体設定からバージョンを確認。



ようやくゲームの吸い出し工程。

そのためにPSP Filerを下記からダウンロード

[PSP Filer v6.6 Download - (PSP, Homebrew Applications) - Brewology - PS3 PSP WII XBOX - Homebrew News, Saved Games, Downloads, and More!](https://www.brewology.com/downloads/download.php?id=11321&mcid=1)


解凍して、
releaseディレクトリのGAMEとGEME150をSDカードのPSPの中にコピー


そして、PSPでPSP Filerを起動。

ファイル一覧が出たら、三角からメニューを開き、RボタンでUMDを吸い出す。
ファイル名は変えてもいいけど、英字にする方法が分からなかったので、そのままがよさそう。

吸い出しが始まると、400MBぐらいの容量でも10分ぐらいかかったかな。
僕が吸い出したのは、俺の屍を越えてゆけと、パタポン2ぐらいだったから、そこまで大きな容量じゃなかったけど、
もし大作を吸い出すときは、容量足りないかも。



吸い出したisoをPCで、PPSSPPに読み込ませれば普通に実行可能。

吸い出しは手間だけど、一度やればいいから、PSPが生きているうちにやりたいゲームは吸い出しておこうかな。

























