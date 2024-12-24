---
layout: post
title: "Macで家庭内LANにMinecraftサーバを立ててSwitchからつなぐ"
tags : [Mac, 環境構築, ゲーム]
date: 2024-12-25 09:03:44
---


前回、下記でUbuntuでMinecraftサーバを立てたけど、
ニュージーランドに引っ越してから、今のところまだパソコンがMacしかないので、重い腰を上げてこっちでも構築してみる。

[Ubuntuで家庭内LANにMinecraftサーバを立ててSwitchからつなぐ](/2024/06/25/construction-of-minecraft-server-in-lan)

なお、構成は最初からFabric版だし、
データも前のを引き継ぐので、基本的には上記記事と同じだが、
特にDNSサーバが違っている。


### 構成

構成図。


![construction-of-minecraft-server-in-lan1-02_01.png]({{ BASE_PATH }}/images/2024/12/25/construction-of-minecraft-server-in-lan-02_01.jpg)


①：DNSサーバに問い合わせ
②：BedrockConnectに接続
③：BedrockConnectで返された入力欄にマイクラサーバのアドレスを入力して接続
という流れは同じ。




### 環境情報

まずは環境情報。
MacBookAir の、2023 M2 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		14.6.1
BuildVersion:		23G93
```

ちなみに、JavaはAmazon Correttoをbrewでインストールした。

```bash
$ brew install --cask corretto

$ java -version
openjdk version "23.0.1" 2024-10-15
OpenJDK Runtime Environment Corretto-23.0.1.8.1 (build 23.0.1+8-FR)
OpenJDK 64-Bit Server VM Corretto-23.0.1.8.1 (build 23.0.1+8-FR, mixed mode, sharing)
```





### FabricとそのModなどの用意


すでにバージョンが1.21.4になっていたので、最新版をダウンロードしてくる。

[Download Minecraft Server Launcher &#124; Fabric](https://fabricmc.net/use/server/)


Modを入れるのに必要なFabric APIは下記から最新版をダウンロード。
[Fabric API - Minecraft Mods - CurseForge](https://www.curseforge.com/minecraft/mc-mods/fabric-api)

GeyserとFloodgateの最新版も下記の各ページにいってダウンロード。
[GeyserMC - Organization](https://modrinth.com/organization/geysermc)


BedrockConnectも対応した版をダウンロード。
[Releases · Pugmatt/BedrockConnect · GitHub](https://github.com/Pugmatt/BedrockConnect/releases)

各ファイルダウンロードしたら、既存jarを置き換え。
1から作る人は前回の記事を参考にしてください。



### DNSサーバの構築


今回のメイン

一応やりたいことは下記で説明済み。

[Minecraftサーバを立ててSwitchからつなぐために、Ubuntu上にLAN内だけのDNSサーバをたてる](/2024/05/28/dns-server-on-ubuntu)

まずはIPアドレスの固定化
今使っているルーターは光回線の会社が貸してくれたtp-linkというルーター。
そこの設定画面で固定化できる。
詳しくは下記。まあ、だいたいどのルーターも同じような感じ。

[TP-Linkの無線ルーターで、特定の端末（パソコンやスマホ）に常に同じIPアドレスを割り当てる方法（DHCPアドレス予約） – 栗太郎Wi-Fiブログ（WiFiルーター、中継機 ： 設定方法＆つながらない対策）](https://kuritaroh.com/2022/06/14/tp_ipaddress_fix_by_dhcp/)


今回は192.168.88.2で割り振った。
このルーターは192.168.88.1なので、88番台を使っているみたい。


次にDNSサーバのインストール。
Macなので、Ubuntuの時と同じようにsystemd-resolvedというわけにはいかないので、
軽量DNSサーバである、dnsmasqをbrewでインストールした。


```bash
brew install dnsmasq
```

下記で設定ファイルを開いて、

```bash
$ sudo vi /opt/homebrew/etc/dnsmasq.conf
```

下記を追記。
これでdnsmasqが該当IPアドレスへの問い合わせを処理してくれる。
127.0.0.1をいれているのは、一応自身を入れておきたかったから。
何かトラブルがおきたとき調べるのに使えるし。
まあ、別に必須ではない。

```bash
listen-address=::1,127.0.0.1,192.168.88.2
```

そして、DNSを解決してほしい内容を下記ファイルを作って配置。

```bash
$ sudo vi /opt/homebrew/etc/dnsmasq.d/minecraft-server.conf
```

内容は、下記の通り。

```bash
address=/geo.hivebedrock.network/192.168.88.2
address=/play.galaxite.net/192.168.88.2
address=/mco.mineplex.com/192.168.88.2
address=/mco.cubecraft.net/192.168.88.2
address=/play.pixelparadise.gg/192.168.88.2
address=/mco.lbsg.net/192.168.88.2
address=/play.inpvp.net/192.168.88.2
```

そして、dnsmasqを動かす。

```bash
sudo brew services restart dnsmasq
Warning: Taking root:admin ownership of some dnsmasq paths:
  /opt/homebrew/Cellar/dnsmasq/2.90/sbin
  /opt/homebrew/Cellar/dnsmasq/2.90/sbin/dnsmasq
  /opt/homebrew/opt/dnsmasq
  /opt/homebrew/opt/dnsmasq/sbin
  /opt/homebrew/var/homebrew/linked/dnsmasq
This will require manual removal of these paths using `sudo rm` on
brew upgrade/reinstall/uninstall.
==> Successfully started `dnsmasq` (label: homebrew.mxcl.dnsmasq)
```

digで動作確認
自身に向けて、geo.hivebedrock.networkを解決してもらう。

```bash
dig @127.0.0.1 geo.hivebedrock.network

; <<>> DiG 9.10.6 <<>> @127.0.0.1 geo.hivebedrock.network
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17146
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;geo.hivebedrock.network.	IN	A

;; ANSWER SECTION:
geo.hivebedrock.network. 0	IN	A	192.168.88.2

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Sun Dec 15 16:06:32 NZDT 2024
;; MSG SIZE  rcvd: 68

```

よしよし、無事に解決している。



### Switchから動作確認

ここまで構築したら、FabricとBedrockConnectを起動して、Switchからつなぐ。
switchには、あらかじめ192.168.88.2をDNSサーバに設定しておく。
2つ目も設定できるので、8.8.8.8 (GoogleのDNSサーバ）にしておく。

マイクロソフトアカウントでログインした状態で、
Multiplayを選び、特集サーバを何でもいいから選んで入る。

そして、BedrockConnectのサーバ選択画面が表示されたら、
Add Serverから、
自前のアドレス 192.168.88.2
ポート番号 19133
を入力して保存し、
そこに接続しにいくようにすれば、無事ログインできる。


ということで、再度子どもといっしょにマイクラができるようになった！
なんだけど、息子は今日クリスマスでサンタさんからもらったカービィ Wiiデラックスに夢中で全然マイクラやろうとしない（笑）




参考
[macで軽量DNSサーバ dnsmasqを使う #Mac - Qiita](https://qiita.com/ryuichi1208/items/4a30ee9d6706d23f9e91)
[Dnsmasq を使って特定の時間帯・サイトへのアクセス制限をする](https://zenn.dev/noraworld/articles/access-restriction-using-dnsmasq#dnsmasq-%E3%81%AE%E8%A8%AD%E5%AE%9A)
[dnsmasq - ArchWiki](https://wiki.archlinux.jp/index.php/Dnsmasq)



