---
layout: post
title: "LinuxでPS2エミュレータの準備"
tags : [Linux, ゲーム]
date: 2022-12-01 10:14:44
---


うちには昔使っていたPS2 薄型があるが、
いつ壊れるかわからないし、それで昔のゲームが遊べなくなるのはちょっとなーということで、
エミュレータを用意することにする。

Windowsでの事例はよく見られるが、
Linuxはあまり見かけなかったので、書いておく。
Ubuntuでやったが、あまりUbuntu依存は少なかったので、
他Linuxでも適度に読み替えればいけると思う。



おおまかな手順は、下記。
一個一個は難しくない。


1. PCSX2のインストール
2. DVD書き込み読み込みツール braseroのインストール
3. USBへBIOS Dumperのコピー
4. DVD-RへFreeDVDBootの書き込み
5. PS2からBIOSの吸い出し
6. BIOSのPCへのコピー
7. ソフトの吸い出し

だから、PS2本体（薄型）、ゲームソフト、USBメモリ、空DVD-Rが必要。
薄型じゃないとだめらしい。
僕はSCPH-70000って型番。これもあまり古いとだめって記述もあるけど、
少なくても70000では上手くいった。


ちなみに参考にした中で、わかりやすかったのは下記
Linuxではないけど、やる内容が簡潔にまとまっていた。

[【動作確認】PS2のBIOSを抽出し、「PCSX2」をインストールする方法を紹介する【Windows】 &#124; 備忘録的メモ帳](https://kakihey.com/pc-gaming/ps2-bios/)



### PCSX2のインストール

PS2のエミュレータといえば、PCSX2一択の状況らしい。


```bash
# PPAを追加
$ sudo add-apt-repository ppa:gregory-hainaut/pcsx2.official.ppa
$ sudo apt update
$ sudo apt install pcsx2

```

ここのwikiを参考にしている。
[Installing on Linux · PCSX2/pcsx2 Wiki · GitHub](https://github.com/PCSX2/pcsx2/wiki/Installing-on-Linux#ubuntu-ppa)



### DVD書き込み読み込みツール braseroのインストール


DVDの読み込み書き込みツールとしてbraseroをインストール
別に書き込みと読み込みができるなら別のでもいい。
braseroは、以前何度か使ったことあるが、簡単で使いやすい。

```bash
$ sudo apt install brasero
```



### USBへBIOS Dumperのコピー

USBの中身が残っていたら、まずはフォーマット 

USBは4GBのを使った。TranscendのJF-V10ってやつ。
容量が大きすぎるとだめって記述を見かけたので、結構古いやつを使う。

Ubuntuなら右クリックでフォーマットを実行、 FAT を選べばいい。


次にBiosをダンプする用のプログラムをダウンロードするが、
色々やり方解説しているサイトのリンクは切れていることが多いので、
僕がつながったのは下記のAttached Filesのところ

[BIOS Dumper v2.0 - BIN](https://forums.pcsx2.net/Thread-BIOS-Dumper-v2-0-BIN)

ダウンロードされたものは、7zで固まっているが、Ubuntuならデフォルトで解凍可能
中に入っている下記4つをフォーマットしたUSBにコピーしておく

* 7za.exe
* DUMPBIOS-MASS.ELF
* ps2dumper.elf
* README.txt




### DVD-RへFreeDVDBootの書き込み


次に、空DVD-Rに焼くisoを、下記のStep1のリンクからダウンロード

[GitHub - CTurt/FreeDVDBoot: PlayStation 2 DVD Player Exploit](https://github.com/CTurt/FreeDVDBoot)


空DVD-Rをいれて、
isoファイルを、右クリックからbraseroで開けば書き込みできる。
設定は特にいじらなかった。
ここで5分から10分ぐらいかかったかな。
ちなみに僕はDVD-RWを使ったが同じだと思う。





### PS2からBIOSの吸い出し

USBとDVD-Rを用意できたら、いよいよPS2からの吸い出し。

まず、表示言語を英語にするために、
コントローラー以外何もつけずにPS2を起動して、表示言語を英語にする。


で、一度スイッチを切って、
USBとDVD-Rをセットし、起動。


灰色画面がでてきたら、
FileBrowserを選択し、
次にmass:/を選択

そのあと
DUMPBIOS-MASS.ELF
を押すことでDUMPが始まる


だいたい、10分ぐらいかかると思う。
画面にDumpingなんちゃらってのがでてくるが、
5つ目の、Dumping NVMがCompleted OKになったら完了の合図。

スイッチを切って、USBとDVD-Rを外す。




### BIOSのPCへのコピー

吸い出されたBIOSがUSBに入っているので、
USBをPCにつなぎ、BIOSのファイル

* .BIN
* .EROM
* .NVM
* .ROM1
* .ROM2
の拡張子を持つファイル5つをコピーし、
~/.config/PCSX2/bios
内に放り込む（ディレクトリはなければ作る

バックアップはとっておいたほうがいいよ。


これでエミュレータ自体の準備は完了。


### ソフトの吸い出し

ソフトの吸い出しは簡単。
ゲームソフトを、DVDドライブにいれたら、braseroで読むこむだけ。
isoファイルとしてPCにコピーされる。
ゲームソフト1本4.7GBあるから、5〜10分ぐらいかかるかもね。






### エミュレータ起動

PCSX2を起動して、
メニューから、CDVDを開いて、吸い出したゲームのisoを選択する。

起動はメニューのシステムから、Boot ISO (fast)で起動。
fullは使わなくてもよさそう。PS2起動画面から起動する場合しか用がない。

ちなみに、僕のPCでは、おそらくCPUが古すぎて、
フレームレートが30を切ってしまうので、まともに遊べなかったが、
動作自体は問題なく動いていた。（試したのはドラクエ8と機動戦士ガンダム 連邦VSジオン DX ）

設定を色々いじっても遅いのはほとんど改善されなかったから、きっとCPU変えなきゃだろうな。
そうなるとマザボも変えなきゃだなー。うーん、どうしよう。




ちなみに、ゲームソフトが対応しているかどうかや、
どういう設定で動かしたかは下記で調べるとよい。

[PS2ソフト動作報告 - プレステ2エミュについて語ろう - atwiki（アットウィキ）](https://w.atwiki.jp/emups2/pages/12.html)


また、古いけど、一度は目を通しておくとよいと言われているのが、下記。

[PCSX2 v0.9.8 日本語版公式設定ガイド](https://forums.pcsx2.net/Thread-PCSX2-v0-9-8-%E6%97%A5%E6%9C%AC%E8%AA%9E%E7%89%88%E5%85%AC%E5%BC%8F%E8%A8%AD%E5%AE%9A%E3%82%AC%E3%82%A4%E3%83%89)
