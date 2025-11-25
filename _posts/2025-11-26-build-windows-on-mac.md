---
layout: post
title: "Mac M4(Silicon) にて、無料でWindowsを動かす"
tags : [Mac, Windows, 環境構築]
date: 2025-11-26 11:34:44
---


長らくWindowsを敬遠していたのだが、
学校の授業でPower BIを使うことになって、
Power BIのブラウザ版だと機能制限があるというので、
思い切って準備してみることにした。

仮想化は、去年無料になったVMware Fusion

Power BIのためだけに、Windowsのライセンスを買うのもなんだかなーって思ったので、
Windows Insider Previewプログラムに参加して、無料でWindowsを使わせてもらう。
もしこれで、後々無料じゃなくなっても、どうせ普段使いしないので、途中で使えなくなってもまあ諦めるぐらいの気持ちではいる。


ということで、やったことを。



### 環境情報

まずは環境情報。
MacBookAir の、2025 M4 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		15.7.2
BuildVersion:		24G325
```


### VMware Fusionのインストール


まずVMware Fusionをインストールする。
 (Proって昔はついていたけど、今はこれが取れて無料でProの機能が使えるみたい)


VMwareがBroadcomに買収されたので、Broadcomアカウントが必要みたい。
gmailだと登録できないという情報もあったが、僕は問題なく登録できた。
流れはちょっと複雑なので、下記を参考にするほうがよい。

[無償化されたVMwareを使用してみる &#124; 株式会社エー・エム・ティー](https://a-m-t.co.jp/blog/vmware-free/)

BroadcomのSupport Portalからアカウント登録。
そのあと、VMwareのサイトのProductから、「SEE DESKTOP HYPERVISORS」をクリックして、
進んでいって、VMware Fusion 13.6.4をダウンロード。

ただし、住所登録はしないといけなかった。
うーん、入れたくはないけど、まあ別にいいか。



### Windows のダウンロード

下記から、Windows Insider Preview プログラムへ登録する。

https://insider.windows.com/ja-jp/about-windows-insider-program/


Microsoftアカウントは元々持っていたものを使った。



で、
MacのSiliconで動かすためには、
ARM64用の実行ファイルが必要なので、下記リンクからダウンロードする。

https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewARM64


Canaryはかなり最先端のビルドらしくて、
流石に安定性に欠けるかなと思い、Devにする。
2025/11/25現在、Build 26200だった

言語はEnglish (United State)しかない。
くそ、せめてUKが良かった。

13GBもあるけど、VHDXファイルがダウンロードできる。
(普通のISOなら8GBでいいのに)




### Qemuにて、ファイルの変換

qemu-imgコマンドにて、
VHDXファイルを、VMware Fusionなどで利用可能なVMDKファイルに変換する。

まずはbrewにて、qemuのインストール


```bash
$ brew install qemu


$ qemu-img --version
qemu-img version 10.1.2
Copyright (c) 2003-2025 Fabrice Bellard and the QEMU Project developers

```


続いてダウンロードしたVHDXファイルを指定して変換をかける

```bash
$ qemu-img convert -f vhdx -O vmdk Windows11_InsiderPreview_Client_ARM64_en-us_26200.VHDX Win11_ARM64.vmdk
```

13GBだけど、数分で終わった。
変換後のファイル名はなんでもよかったが、わかりやすい感じに。



### VMware Fusionで動かしてみる


この動かすところで少しハマった。
どこから起動すればVMDKファイルを指定すればいいのか分からない。
で調べてみると、
下記の記事が見つかり、


[待望のM1 Mac対応のVMware Fusion Tech Previewを試してみた！ コイツ、Windows11 Insider Previewも動くぞ。 - とむむの日々](https://tomm.hatenablog.com/entry/2021/09/24/120000#Windows11-Insider-Preview%E3%82%92%E5%8B%95%E3%81%8B%E3%81%99)



Windowsとしてではなく、Other 64 ARMとして起動する必要があるらしい。


下記の流れ。


![build_windows_on_mac_01]({{ BASE_PATH }}/images/2025/11/26/build_windows_on_mac_01.png)


![build_windows_on_mac_02]({{ BASE_PATH }}/images/2025/11/26/build_windows_on_mac_02.png)


![build_windows_on_mac_03]({{ BASE_PATH }}/images/2025/11/26/build_windows_on_mac_03.png)


この最後で、Choose virtual disk ボタンからVMDKファイルを選択して先に進む。
名前がOther 64 ARMで起動しちゃうが、あとで変えることができる。

最初は、プロセッサーは2つ、メモリは4GBにして起動したが、
あとでPower BIが上手く動くようになんどか試して、プロセッサー4つ、メモリ8GBにした。



で、起動すると、Windowsのインストール画面が出るので、
国や言語を選んだ後、

Wifiマークがでて、
「Let's connect you to a network」と表示される。
で、ここで上記の記事では、"I don't have internet"を押せと書いてあるのだが、
これが出てこない。
どうしたものかと思って調べると、redditにて、

[Unable to connect to network (Apple Silicon) : r/vmware](https://www.reddit.com/r/vmware/comments/1bjlrdm/unable_to_connect_to_network_apple_silicon/)


Wifiマークの画面で、Shift + F10 を押してコマンドプロンプトを起動。
そして、そこで、
```
$ oobe\bypassnro
```
を実行する。

すると、ここで"I don't have internet"が押せるようになる（もしかしたら一回再起動したかも。）ので、
それから先に進む。


その後はもうインストール完了まで特に問題はなかった。


インストールした後、インターネットに繋がらないので、
下記記事を参考にした。

[M1 Mac対応のVMware Fusion Tech PreviewでWindows11もネットワーク接続できるぞ！（追記：サウンド有効化、CD/DVD利用も可能に！） - とむむの日々](https://tomm.hatenablog.com/entry/2021/09/30/213000)


上記記事ではコマンドプロンプトを管理者権限で起動しているが、
たまたまPowerShellが管理者権限で開いたのでそっちで。
下記を実行
2つ目のコマンドでなんかKeyが表示されるが、特に覚えておく必要はなさそう。

```
$ bcdedit /debug on

$ bcdedit /dbgsettings net hostip:10.0.0.1 port:55555

$ shutdown -r -t 0
```

再起動すると無事にネットに繋がる。



で、画面サイズが上手く合わないと思っていたのだが、
Windows起動している状態で、
下記のように、Reinstall VMware Toolsを押す。
(Reinstallと書いてあるが、まだ一度もインストールしたことない。まあとりあえず押す)

![build_windows_on_mac_04]({{ BASE_PATH }}/images/2025/11/26/build_windows_on_mac_04.png)

すると、Dドライブにマウントされるので、
Windows上でツールインストールを実行。

終わると自然と画面サイズが調整できるようになる。
Windowsのディスプレイ設定でもいじれるからお好きなサイズに。


あとは、Macからのファイル共有かな。
下記を参考に、ファイル共有ONにしてWindowsと共有できた。

[VMware Fusion 13.5 M2 MacとWindows11のファイル共有](https://www.maclab.tokyo/document/vmware-fusion-13-5-share/12451/#google_vignette)






Microsoft Storeから、Power BIを探してインストールし、無事に動かすことが完了。

ふー。いけるもんだな。普段遣いしないならこれで十分そう。







参考

[無償化されたVMwareを使用してみる &#124; 株式会社エー・エム・ティー](https://a-m-t.co.jp/blog/vmware-free/)
[VMware Fusion Proをインストールする &#124; Flake’s Stack](https://salmonflake.net/install_vmware_fusion/)
[Windows PCはもうすぐMacになることが出来なくなるが、MacはWindows PCにだってなれる（ARM版Windows 11 Insider Preview CanaryをVMware Fusion環境に導入したお話） - シン・ひよりんだいとぅきおちゃんぎんらんど](https://kanoayu.cloudfree.jp/2023/11/27/windows-pc%E3%81%AF%E3%82%82%E3%81%86%E3%81%99%E3%81%90mac%E3%81%AB%E3%81%AA%E3%82%8B%E3%81%93%E3%81%A8%E3%81%8C%E5%87%BA%E6%9D%A5%E3%81%AA%E3%81%8F%E3%81%AA%E3%82%8B%E3%81%8C%E3%80%81mac%E3%81%AFwind/)
[M1 Macに無料でWindows 11 Proをインストールした話。](https://tabkul.com/?p=262188)
[Tableau DesktopがAppleシリコン対応アプリじゃない件　その3｜th_tableau](https://note.com/th_tableau/n/nfa486e778d68)
[待望のM1 Mac対応のVMware Fusion Tech Previewを試してみた！ コイツ、Windows11 Insider Previewも動くぞ。 - とむむの日々](https://tomm.hatenablog.com/entry/2021/09/24/120000#Windows11-Insider-Preview%E3%82%92%E5%8B%95%E3%81%8B%E3%81%99)
[M1 Mac対応のVMware Fusion Tech PreviewでWindows11もネットワーク接続できるぞ！（追記：サウンド有効化、CD/DVD利用も可能に！） - とむむの日々](https://tomm.hatenablog.com/entry/2021/09/30/213000)
[Unable to connect to network (Apple Silicon) : r/vmware](https://www.reddit.com/r/vmware/comments/1bjlrdm/unable_to_connect_to_network_apple_silicon/)
[VMware Fusion 13.5 M2 MacとWindows11のファイル共有](https://www.maclab.tokyo/document/vmware-fusion-13-5-share/12451/#google_vignette)
