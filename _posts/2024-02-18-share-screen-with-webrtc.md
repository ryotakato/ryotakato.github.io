---
layout: post
title: "WebRTCを使って画面共有を実装してみた"
tags : [Node.js]
date: 2024-02-18 08:48:44
---

今回、WebRTCという技術を使って、PC画面の共有をやってみた。
といってもローカルネットワークで動くだけのもの。

つくったものは下記。

[ryotakato/browser_viewer](https://github.com/ryotakato/browser_viewer)




きっかけは、
リビングでAmazon Primeを見るためのMacがあるのだけど、それが見れなくなったから。
Chromeを更新しろというのだけど、更新が上手くいかない。
ブラウザとしては使えるけど、妻も子供もPrimeやYouTubeを見るためのものとして使っていたから、Primeが見れないのはツラい。
かと言って、僕のPCで見るには部屋の環境が良くない。（この部屋にイス一つしかないし。）

ということで、僕のPC（Linux）でPrimeを再生し、
その画面自体をChrome越しでリビングのMacに飛ばして、観ることはできないかと考えたわけだ。



色々調べると、WebRTCという技術があって、
それが今回の目的には最適だと思った。
最近はWebTransportってのもあったけど、
まだ新しいのか、どう使うのかの情報が少なかったから、今回はパス。
そこらへんは下記が詳しい。（続き書いてくれないかなー。
[WebRTC徹底解説](https://zenn.dev/yuki_uchida/books/c0946d19352af5)


WebRTC自体はP2Pの技術なので、ブラウザ2つだけあればいいのだが、
とはいっても、動画を共有するまでの手続きはサーバー（シグナルサーバー）があったほうがやりやすいので、
とりあえず下記を参考に、
Node.jsのSocket.ioとExpressでWebRTCに必要な処理を作ってみた。

[PCの画面をリアルタイム配信 WebRTC 1日目 #JavaScript - Qiita](https://qiita.com/kotazuck/items/7ad1672c71aa38d6af6d#%E9%85%8D%E4%BF%A1%E5%81%B4)
[WebRTC を理解するためにカメラ映像を送るだけの最小実装を探る &#124; blog.ojisan.io](https://blog.ojisan.io/webrtc-video-minimal-impl/)
[【WebRTC学習】② ローカルPCだけでP2Pの接続を実装する - It’s now or never](https://inon29.hateblo.jp/entry/2020/02/03/221103)
[WebRTCを実装してみた #Node.js - Qiita](https://qiita.com/Turtle-child-No2/items/7205d5c1399375a8c10b)

僕の構成では、

```bash
$ node signal_server.js
```

でサーバーを動かした後に、
1. 配信側が、send.htmlにアクセスして、roomIdを決める（パスワードの代わりのつもり）
2. 受信側が、receive.htmlにアクセスして、roomIdを入力する
3. 配信側が、broadcastボタンを押して、画面共有を開始する
4. 受信側が、receiveボタンを押して、画面共有を見る
という手順。
あ、Chrome前提だし、配信側は3のタイミングで、他画面とかじゃなくてChromeのタブを選択しないと音声が飛ばせないので注意。
あと、ローカルネットワークしか対応していない。

基本機能は1日ぐらいでできて、僕のLinuxの画面をリビングで見れるようになった。
（ここで大変だったのは、動作確認のために何度もリビングと部屋を移動しないといけなかったことｗ）

この方式のいいところは、配信側は配信しつつもPCを使えるところ。専有にならないので、配信側が不便を感じることはない。



じゃあ、実際に動画を見てみようと思ってやってみると、
受信側の映像が、そこそこカクカクした感じになる。
見れないことはないんだけど、コマ送りみたいになって、
映画を1本これで見るのは、目が疲れるなと思った。

きっとフレームレートが低いのだろうなと思って、

[SkyWayトラブルシューティング入門 #WebRTC - Qiita](https://qiita.com/B-B/items/25603041288271670c6e)
[WebRTCデバッグ入門 #WebRTC - Qiita](https://qiita.com/yusuke84/items/8d232c8d24156f16e8ba)


などを参考にして、
chrome://webrtc-internals/

を使って見てみると、確かにフレームレートが20ぐらいか、下手したらもっと低い。

たぶん、WebRTCの帯域制御などに詳しくなればもっと色々できるんだろうけど、
今回はそこまで学ぶのが時間なかったので、
とりあえず解像度を下げることで、その分をフレームレートあげることはできないかと考え、
配信側の、getDisplayMediaの呼び出しのところで、解像度とフレームレートの理想値を指定してみた。
参考にしたのは下記。

[映像や音声を取得し、RTCPeerConnectionに与える](https://zenn.dev/yuki_uchida/books/c0946d19352af5/viewer/320c67)



すると、受信側のフレームレートが40〜50ぐらいまで改善し、
映画を観るのに支障がないぐらいにはできた。
解像度は下げたが、配信側を全面表示にしてやれば、そこまで気にならない。

これで、子供が今ハマっているハリーポッターを見せてあげることができそう。
いやー、良かった。
僕の技術が家族の役に立ったのは珍しいｗ


すぐではないが、WebTransportなども学んでいきたいなー。







