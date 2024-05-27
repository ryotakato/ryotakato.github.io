---
layout: post
title: "Minecraftサーバを立ててSwitchからつなぐために、Ubuntu上にLAN内だけのDNSサーバをたてる"
tags : [Linux, 環境構築, ゲーム]
date: 2024-05-28 00:28:44
---


ローカルPCにMinecraftのサーバを立てて、
Nintendo Switchからアクセスしてできるようにしたかったんだけど、
SwitchはDNSサーバを騙さないと、繋げないということだったので、
ローカルPCであるUbuntu 22.04LTSに、
簡易DNSサーバを立てて、そこにアクセスしてもらうようにする。
Minecraftサーバ立てたり、実際につなぎにくるまでの詳細は別途書く予定。


### 環境情報


```bash
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.4 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```


### IPアドレスの固定化

DNSサーバ化するには、
IPアドレスは固定じゃないと色々やりにくい。
（いちいちSwitchのDNS設定を変えるのが面倒）


なので、固定化したいが、
Ubuntu上でやるとなるとnetplanなどで設定しなくてはいけなくて、よく分からなかったので、
Wifiルーター上で設定した。

うちはNuro光なので、ルーターはF660A。
なので、下記を参考にMacアドレスから、UbunutのIPアドレスを192.168.1.8に固定した。


[【NURO光ONU】F660AでサーバやGoogleHomeのIPを固定化する - がらぱっぱ](https://garapappa.hatenablog.com/entry/20210305/1614874424)




### DNSサーバ


といっても、元々Ubunut 22.04LTSに入っているsystemd-resolvedを使う。
ネットを色々探すと、dnsmasqやBind9、Unboundなどを使う情報が溢れかえっているが、
どれもsystemd-resolvedがすでに使っているポート番号53と被るので、
結局systemd-resolvedを止めたりしないといけないみたい。

だけど、そんなに大掛かりにやりたくなかったし、
そもそも、他のを入れなくてもsystemd-resolved自体に名前解決できる機能がすでにあるので、
それをどうにかできないかを検討した。


色々調べて、DNSStubListenerExtraというのを使えばいいことが分かった。

これは、デフォルトではローカルPC用でしかないsystemd-resolvedのスタブリゾルバを、
指定したIPアドレスでも待ち受けるようにするもの。

ということで、systemd-resolvedの設定ファイルである、下記を編集。

```bash
$ sudo vi /etc/systemd/resolved.conf
```




```bash
DNSStubListenerExtra=192.168.1.8
```

を追記して、


```bash
$ sudo systemctl restart systemd-resolved
```

で再起動。


参考
[resolved.conf](https://www.freedesktop.org/software/systemd/man/latest/resolved.conf.html)
[Debianでルーターを構築 #IPv6 - Qiita](https://qiita.com/m-m-n/items/0fa1bc14ed6584295adc#dns%E3%81%AE%E8%A8%AD%E5%AE%9A)



### Minecraftのためにドメインを設定

Minecraftの統合版の特集サーバと呼ばれるもののドメインを、
上記DNSサーバを使って、DNSサーバと同じマシンに誘導するため、下記を/etc/hostsファイルに記載。


```bash
#minecraft
192.168.1.8    geo.hivebedrock.network
192.168.1.8    play.galaxite.net
192.168.1.8    mco.mineplex.com
192.168.1.8    mco.cubecraft.net
192.168.1.8    play.pixelparadise.gg
192.168.1.8    mco.lbsg.net
192.168.1.8    play.inpvp.net
```


```bash
$ sudo systemctl restart systemd-resolved
```

で再起動。



### ファイヤーウォールでポート開放

LAN内のローカルネットワークとはいえ、
ポート開放は必要なので、
DNSサーバのポートである53を開放する。
tcpは使ってないと思うので、udpだけ。


```bash
$ sudo ufw allow 53/udp
```


設定を再読込

```bash
$ sudo ufw reload
```


なお、ポート開放はちょっとドキドキするが、
LANの外にまで開放するわけじゃないから、よしとする。




### 動作確認

これで、設定ができたので、
同じLAN内の、別マシンから確認してみる。
今回はMacからつないでみた。


```bash
$ dig @192.168.1.8 geo.hivebedrock.network
```

と打ったときに、


```bash
〜中略〜

;; ANSWER SECTION:
geo.hivebedrock.network. 0	IN	A	192.168.1.8

;; Query time: 0 msec
;; SERVER: 192.168.1.8#53(192.168.1.8) (UDP)

〜中略〜
```

と出れば成功。




色々調べたおかげで、少ない設定ですんでよかった。




