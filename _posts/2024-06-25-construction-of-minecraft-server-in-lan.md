---
layout: post
title: "Ubuntuで家庭内LANにMinecraftサーバを立ててSwitchからつなぐ"
tags : [Linux, 環境構築, ゲーム]
date: 2024-06-25 07:38:44
---


### きっかけ

子供と遊ぶために、Minecraftを買った。
ずっと買おう買おうと思っていて、ようやく子供の誕生日に買ったというわけ。

Minecraft自体は僕は10年前にやっていて、
別にエンダードラゴン倒すとかまでやり込んでたわけじゃないけど、
好きなもの作って、好きに遊ぶってことをやってたので、
LEGO好きな子供にもやらせてあげたいと思ってた。

Switch版を買って、子供一人で遊んでいたけど、やっぱり一緒が楽しいよねってことで、
2人プレイを調べていたんだけど、
Switchだと、テレビを半分割して、それぞれコントローラーを用意すればできるということなのだが、
ウチのテレビは小さいので、半分割には向いてないし、
コントローラーもおすそ分けプレイではできないとのことなので、新しくコントローラーを買う必要があるのだが、
それはそれで出費が痛い。

ということで、
僕の持っているJava版Minecraftとマルチプレイできないかという風に考えた。
これができれば、僕はパソコンで、子供はSwitchで、一つのワールドに入って一緒に遊べると考えたわけだ。



### 構成

ということで、まずは完成した後の構成図から。


![construction-of-minecraft-server-in-lan_01.png]({{ BASE_PATH }}/images/2024/06/25/construction-of-minecraft-server-in-lan_01.png)


緑と青の矢印は、
それぞれJava版と統合版からアクセスする経路を示している。

統合版で、かつSwitchのアクセスは、実際には①、②、③のように接続される。
①：DNSサーバに問い合わせ
②：BedrockConnectに接続
③：BedrockConnectで返された入力欄にマイクラサーバのアドレスを入力して接続
という流れ。
なぜこんなことが必要なのかは、構築の中で書いていく。

参考にしたのはこれ。
この記事ではRaspberryPiだが、これができるなら、1台のPCでもいけると思ったのがきっかけ。
[マイクラ統合版のサーバを自分で立ててSwitchから接続する方法 #RaspberryPi - Qiita](https://qiita.com/manontroppo1974/items/78f4b1b8d81694f9fd4a)

[【マイクラ統合版】Switchから外部サーバーへ接続する方法](https://www.radical-dreamer.com/game/minecraft_bedrockconnect/)


### 環境情報


環境情報。
ちなみに、Javaは、すでに入れてある。
詳しくは前々回の下記を参考
[Ubuntu 22.04LTSに、OpenJDKであるAmazon Corretto 21をインストール](/2024/05/26/install-openjdk-amazon-corretto-21)


そして、これを実施した時点では、Minecraft自体は1.20.6バージョンだった。


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



### Paper MCのインストール

MinecraftのサーバにはPaperMCを選んだ。
どうせ子供とはバニラで遊ぶので、特にこだわりはないのだが、
最終構成になるようにするには選択肢が少なかったし、
情報が色々でてきたので、PaperMCでいいやってなった感じ。


やったのは下記Getting Startedに従って、jarをダウンロードしただけ。

[Getting Started &#124; PaperMC Docs](https://docs.papermc.io/paper/getting-started)

1.20.6のpaper.jarをダウンロードし、
適当なディレクトリ（ここでは、PaperMCディレクトリ）に配置したら、
下記コマンドで一旦動かす。




```bash
$ cd PaperMC
$ java -Xms4G -Xmx4G -jar paper.jar --nogui
```

すると、
「You need to agree to the EULA in order to run the server. Go to eula.txt for more info.」
的なメッセージが現れて起動がストップするので、

自動で作られた、eura.txt の中を、
「eula=false」から「eula=true」に変更する。

これで再度起動すると問題なく動く。
なお、難易度などはserver.propertiesで変更できるが、今回は特に変更しなかった。

また、今後何度も起動と停止をすることになるので、下記で起動スクリプトを作っておくとよい。
メモリは、2人でやる分には4GBもいらず、2GBで十分。
[Start Script Generator &#124; PaperMC Docs](https://docs.papermc.io/misc/tools/start-script-gen)


参考
[Minecraft: 統合版クライアントともクロスプレイできるJava版サーバの構築方法など - RemoteRoom](https://remoteroom.jp/diary/2022-11-14/)



### ローカルから動作確認

Ubuntuに入れているMinecraft LauncherからMinecraftを起動し、
Multiplayのほうから進んで、
localhost:25565
でアクセスすると、世界が構築されて、
いつもどおりにMinecraftが楽しめる。

ということで、ローカルからは問題ないので、
PaperMCは問題なかった。



### Geyserプラグインと、Floodgateプラグインのインストール

ここで、2種のプラグインを導入する。
Geyserは、統合版からJava版へ接続するときの変換処理を担当するプラグイン。
PaperMCはJava版（というか統合版で動かす自前サーバって聞いたことない）ので、Switchから接続するには変換が必要になる。
なお、Java版に接続しにいくとき、普通はJava版のアカウント（マイクロソフトアカウントとは別）が必要になるのだが、
それを不要にするのが、Floodgateプラグイン。
たぶん何かの偽装をしているのだろうと思う。

さて、この2つをインストールする。
下記のGeyserタブとFloodgateタブから、それぞれ「Spigot/Paper」を選択するとダウンロード開始。
この2つのjarを、PaperMCを入れたディレクトリのpluginsディレクトリ内に配置する。


[GeyserMC: Download](https://geysermc.org/download)


そして、PaperMCを再起動。
ログにプラグインの読み込みなどが表示されるが、
落ち着いたら（5分はまったと思う）、停止する。


そうすると、下記の配下にそれぞれconfig.ymlができている。
PaperMC/plugins/Geyser-Spigot/
PaperMC/plugins/floodgate/


floodgateのほうは変更なしでいいが、
Geyser-Spigotのほうは、下記2つを変更しておく。

```bash
bedrock.port: 19133

remote.auth-type: floodgate
```

ポートを19133に変えているのは、後でBedrockConnectを19132で動かすため。


変更したら、再度PaperMCを起動。


ちなみに、設定項目の説明などは下記参照
[マイクラ統合版とJava版の完全なクロスプレイを実現する方法 &#124; Novaの日記](https://novablog.work/minecraft-crossplay/)


### LAN内のiOSのMinecraftから動作確認


スマホのiPhoneにもMinecraftをいれていたので、
ここで、動作確認してみようと思った。

で、スマホからサーバのアドレスとポート番号を入れて接続しても、
「世界に接続できませんでした」
って出るばかりで、一向に繋がらない。

Geyserのほうに問題あるのかって思っていたから、

[GeyserMC Wiki: Fixing 'Unable to Connect to World'](https://wiki.geysermc.org/geyser/fixing-unable-to-connect-to-world/)

とかを見て、色々やってたんだけど、全然ダメ。


で、視点を変えてみて、iPhoneのほうを疑ってみたら、
設定の「Minecraft」にある、
「ローカルネットワーク」がONになっていないだけというオチ。
これ、デフォルトはOFFなんだな。

ここを変えれば、通常通り接続することができた！



### BedrockConnectのインストール


そもそも、今回の構成でBedrockConnectとこの次のDNSサーバの構築が必須かといったら、そうではない。

下記の記事で紹介されている通り、
SwitchのDNS設定を変えて、有志で立てられているBedrockConnectに接続しにいけばいいからだ。
実際これでも実現できることは確認済み。

[【マイクラ統合版】Switchから外部サーバーへ接続する方法](https://www.radical-dreamer.com/game/minecraft_bedrockconnect/)

ただ、僕としては、
海外サーバに接続にいくのがちょっとセキュリティ的に不安だったし、
そもそも家庭内のMinecraftに接続しにいくのに、
何故外のWANに出ないといけないのかっていうところが引っかかったため、
自前でBedrockConnectを立てることにした。


インストール自体は簡単。
下記をダウンロードして、適当なところに配置しただけ（今回は、BedrockConnectディレクトリにした）

[Releases · Pugmatt/BedrockConnect · GitHub](https://github.com/Pugmatt/BedrockConnect/releases)


起動スクリプトはこんな感じ。

```bash
$ cat bedrockconnect.sh 
#!/usr/bin/env sh

java -jar BedrockConnect-1.0-SNAPSHOT.jar nodb=true
```

基本これは19132で立ち上がるので、
このために、Geyserのポートを19133に変えたわけだ。


参考（Windowsの記事だけど）
[【改造なし】Xbox One、スイッチ、PS4で特集サーバー以外に接続する方法 &#124; Novaの日記](https://novablog.work/be-join-any-server/)


### DNSサーバの構築

これについては前回の記事を参照のこと

[Minecraftサーバを立ててSwitchからつなぐために、Ubuntu上にLAN内だけのDNSサーバをたてる](/2024/05/28/dns-server-on-ubuntu)


### Switchから動作確認

ここまで構築したら、PaperMCとBedrockConnectを起動して、Switchからつなぐ。

マイクロソフトアカウントでログインした状態で、
Multiplayを選び、特集サーバを何でもいいから選んで入る。

そして、BedrockConnectのサーバ選択画面が表示されたら、
Add Serverから、
自前のアドレス 192.168.1.8
ポート番号 19133
を入力して保存し、
そこに接続しにいくようにすれば、無事ログインできる。


これでようやく子供とマルチプレイができるようになった！
やっぱり二人でやったほうが楽しいし、
助け合えるから、まだマイクラに慣れていない子供でもドンドン進めていける。




### 後日談

と、ここまで書いたPaperMCの構成で遊んでいたんだけど、
10日ぐらいたったとき、1.21のアップデートがきた。
Switchと、PCのMinecraft Launcherは自動アップデートだから、勝手にアップデートされてしまったので、
そこからPaperMCに接続しようとしたけど、バージョンが古くてダメだった。

そっかー、アップデートに追随しないといけないよなって思って、
PaperMCをアップデートしようと思ったら、まだ出てないの。
必ずしもすぐに対応できるわけじゃないのは、有志で作っているからで、
それ自体は仕方ないと思うのだけど、
子供と遊ぶ時間が限られている身としては、やりたいときにすぐできないのはキツいなと思った。
自動アップデートを抑制するのも考えたけど、でもやっぱり最新のアップデートの内容は楽しみたいと思ったし。

こんだけMinecraftが流行っているのだから、即日対応のサーバもあるだろなと考え、
Geyserとfloodgateが使える構成のサーバ（だからこの2つが使えない公式サーバは選択肢から外した。）を探してみると、
Fabricが良さそうだった。

[Fabric &#124; The home of the Fabric mod development toolchain.](https://fabricmc.net/)


すぐに最新バージョンにも対応してそうだったので、
MinecraftFablicServerディレクトリを作って、そこにjarをダウンロード。

PaperMCのStart Script Generatorで作ったスクリプトをそのまま持ってきて、jar名だけ差し替える。

一度起動した後、eula.txtを同じようにtrueに変える。


Geyserとfloodgateはプラグインじゃなくて、MODとしていれることになっている。
しかも、FabricでMOD使うためには、Fabric APIというMODが必要なので、
それもダウンロードしてくる。
下記がリンク。

[Fabric API - Minecraft Mods - CurseForge](https://www.curseforge.com/minecraft/mc-mods/fabric-api)

[GeyserMC: Download](https://geysermc.org/download)

GeyserとfloodgateはFabricのものを選ぶ。


で、全部MinecraftFablicServer/modsディレクトリに入れたら、
起動して、また止めて、

MinecraftFablicServer/config/Geyser-Fabric
MinecraftFablicServer/config/floodgate

の配下にconfig.ymlがあるので、
PaperMCとときと同じように編集。

BedrockConnectは構成はそのままでよいが、最新版をダウンロードしてくる。


PaperMCの際のセーブデータを移行したいので、

PaperMC/world
PaperMC/world_nether
PaperMC/world_the_end

のディレクトリをコピーして、
MinecraftFablicServerディレクトリ配下に置いた。
もちろん、元々MinecraftFablicServer配下にあったworldディレクトリは邪魔なので、削除しておいた。


これで、FabricとBedrockConnectを起動してみると、
PaperMCのときと同じように遊べる！

良かったー。



参考
[【プラグイン紹介】統合版とJava版でクロスプレイできるプラグイン【GeyserMC】 - 揚げポテほかほかクラフト](https://agepote.jp/mcplugin/geysermc)





