---
layout: post
title: "Wifiアダプターのドライバをインストール"
tags : [Linux, 環境構築]
date: 2023-08-01 12:15:44
---


この前、普段使っているUbuntu 22.04LTSで、
突然インターネットが切れてしまった。

色々みたところ、外付けWifiが反応してなさそう。
LogitecのSky Linkってやつだけど、まあ、もう古いよね。
Logitecはもう今は外付けWifi出してなさそうだし。

ということで、新しいのを買った。

<div class="AmaQuick-box" style="margin-bottom: 0px;"><div class="AmaQuick-image" style="float: left; margin: 0px 12px 1px 0px;"><a href="https://www.amazon.co.jp/dp/B0B33ZP7YQ/?tag=tavi06-22" name="AmaQuicklink" target="_blank"><img src="https://m.media-amazon.com/images/I/21i01ZVuzGL._SL200_.jpg" alt="バッファロー WiFi 無線LAN 子機 USB2.0用 11ac/n/a/g/b 433Mbps ビームフォーミング機能搭載 日本メーカー WI-U2-433DHP/N" style="border: none;"/></a></div><div class="AmaQuick-info" style="margin-bottom: 10px; line-height: 120%"><div class="AmaQuick-name" style="margin-bottom: 10px; line-height: 120%"><a href="https://www.amazon.co.jp/dp/B0B33ZP7YQ/?tag=tavi06-22" name="AmaQuicklink" target="_blank">バッファロー WiFi 無線LAN 子機 USB2.0用 11ac/n/a/g/b 433Mbps ビームフォーミング機能搭載 日本メーカー WI-U2-433DHP/N</a><div class="AmaQuick-powered-date" style="font-size: 80%; margin-top: 5px; line-height: 120%">posted with <a href="https://creazy.net/amazon_quick_affiliate" title="AmaQuick" target="_blank">AmaQuick</a></div></div><div class="AmaQuick-detail">バッファロー</div><div class="AmaQuick-sub-info" style="float: left;"><div class="AmaQuick-link" style="margin-top: 5px"><a href="https://www.amazon.co.jp/dp/B0B33ZP7YQ/?tag=tavi06-22" name="AmaQuicklink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="AmaQuick-footer" style="clear: left"></div></div>


アンテナ式じゃなくても良かったんだけど、
まあ、デスクトップPCだし、小型じゃなくて良かったので、これを選んだ。

で、当然ながら指しただけではドライバがないので反応しない。

そこで、有線LANを接続して、
ドライバを探すんだけど、一発ではみつからず、ちょっと手間取った。

[Ubuntu 22.04のWIFIドライバ設定（rtl8812au） &#124; てくてくテック](https://tek2tech.com/wifi-driver-install-on-ubuntu2204/)

結果的に上記サイトに書いてある、2,3をやったらできた。


まず2
```bash
$ sudo apt update

$ sudo apt install rtl8812au-dkms
```

ここで、再起動でもダメだった。
設定にWifiが出てこないからね。

次に3

```bash
$ git clone https://github.com/aircrack-ng/rtl8812au.git
$ cd rtl8812au
$ sudo make dkms_install
```

ここで再起動したらWifiが設定できるようになっていたので、選ぶ。
このとき、一度KnownとなっていたESSIDを一度Forgetしておくと繋がった。


アンテナ式だから速度早いかなと思ったけど、今のところ大きな変化は見られず。
とりあえずネット繋がって良かった。



