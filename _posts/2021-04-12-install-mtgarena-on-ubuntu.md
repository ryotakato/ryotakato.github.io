---
layout: post
title: "MTG Arena をUbuntuにインストール"
tags : [Linux, Magic The Gathering]
date: 2021-04-12 09:40:44
---






### きっかけ

子供のころ、Magic: The Gathering をやってた。
テンペストやエクソダスのときなので、第５版ぐらいかね。
メルカディアン・マスクスあたりまでは覚えがあるし、たぶんカードも残ってる。


大人になって、いつか再開したいなとは思ってたんだけど、
紙カードを買っても近くで一緒にできる友達ももういないし、
ならオンラインなんだけど、まだそのときはオンラインでできる環境がまともに整ってなくて(MOってやつに手を出してたけど、UIがひどかったような覚えが)、諦めてた。
しかもプレインズウォーカーとか色々新しい要素が多くて、敬遠してた。

そこに満を持して、MTG Arenaが出たのだけど、
PC版、しかもWindowsやMacしか対応してないのでうーんって思ってた。

しかし、ようやく3/25にiPhone版が出たので、
ダウンロードして遊んでいる。
やっぱり面白いよね。

新しい要素は色々学んでいるところなので、
まだまだ分からないところも多いが、
それでも、楽しめている。
プロレベルになるためにやるわけじゃないので、あまりランキングとか気にせず、
自分でデッキ作って対戦してっていうのをメインに遊んでいこうと思う。


で、iPhone版で全然問題ないのだけど、
デッキ構築のときだけ携帯の小さい画面ではやりづらい。
なので、そこだけでも大きい画面でやりたくて、
Linuxでできる方法を探した。

結果、デッキ構築や対戦は普通にできるようになったので、記しておこうと思う。
Lutoris + Wineを使う方法。
同じようにやるときは自己責任で。





### 環境

* Ubuntu 20.04 LTS
* GPU GeForce GTX 1050

別のディストリビューションでもいけると思う


また、下記手順でインストールした後の各バージョンは、
* GPU ドライバー  NVIDIA 460.39
* Lutoris 0.5.8.3
* Wine wine-5.0.r0.g3ac1519f (LutorisでのMTG Arenaインストール時に勝手に入るバージョン)
* MTG Arena (最新の 2021.03.30版)


### 手順

おおまかにいうと、
* Lutorisインストール
* ドライバーアップデート
* MTG Arena インストール
なんだけど、紆余曲折あったので、
それも併せて記載する。

#### 1. Lutorisインストール

LutorisはLinuxでのゲームプラットホーム。Steamみたいなもの。
Wineを利用して、Windowsのゲームもできる。
ものによっては動かないのもあるらしいので、
公式サイトで確認するといい。
例えば、MTG Arenaなら、

[Magic: The Gathering Arena - Lutris](https://lutris.net/games/magic-the-gathering-arena/)

って感じで、
WINE Latest Self-Updating version
ならGoldバッチがついてて、動作が確認できているとのこと。

Lutorisのインストールはaptで。

```bash
# リポジトリ追加
$ sudo add-apt-repository ppa:lutris-team/lutris

# アップデートして最新とっておく
$ sudo apt update

# インストール
$ sudo apt install lutris
```

ここは特に問題なく。


参考(別に原神をやりたいわけじゃなかったけど、わりと新しめの記事だったので。他ディストリビューションも載ってる。

[Linuxでゲームを動かそう ～Lutrisで原神を動かした話～ &#124; FascodeNetwork Official Blog](https://blog.fascode.net/2021/03/03/lutris_genshin/)



#### 2. ドライバーアップデート

GPUはGeForce GTX 1050を使っているので、
NVIDIAのドライバーバージョン確認したら、390だった。
これでも動くかもしれないけど、どうせなら最新のドライバーにする。


```bash
# 最新が460だった
$ sudo apt install nvidia-driver-460

# Lutorisではvulkan1 APIっての使っているから、入ってなかったら、いれておく。（僕はすでに入ってた）
$ sudo apt install libvulkan1 libvulkan1:i386
```

これも特に問題ないが、
ドライバー変えているので、一応ここで再起動しておいた。



#### 3. WineHQインストール(実際は不要)

で、この次。
Lutorisでゲームインストールすると、必要なWineは自動でインストールしてくれるから、
この手順は不要なんだけど、
なんかどっかで、MTG Arenaは別途Wineの最新版が必要だという記事を見かけたような気がして、
インストールしなきゃって思ってた。

結論からいうと、不要だったので、
MTG Arenaをやりたいだけなら飛ばしてもらって大丈夫。

まあ、Wineの最新版を別途もっておくのも今度役立つかなと思って、入れたWineはそのままにしてある。


```bash
# 32bitアーキテクチャーの機能を有効化
$ sudo dpkg --add-architecture i386

# リポジトリキー追加とリポジトリ追加
$ wget -nc https://dl.winehq.org/wine-builds/winehq.key
$ sudo apt-key add winehq.key
$ sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/' focal main

# 終わったら一応キーを消しておく
$ rm winehq.key

# 安定版をインストール
$ sudo apt install --install-recommends winehq-stable

# バージョン確認
$ wine --version
ERROR: ld.so: object 'libgtk3-nocsd.so.0' from LD_PRELOAD cannot be preloaded (cannot open shared object file): ignored.
ERROR: ld.so: object '/usr/lib/x86_64-linux-gnu/libgtk3-nocsd.so.0' from LD_PRELOAD cannot be preloaded (wrong ELF class: ELFCLASS64): ignored.
wine-6.0 

```
バージョン確認のところで、
ERRORが２つでているが、
ignoredと書いてあるし、今のところ支障はないので、そのままにしてある。
解決策探したけど、あまり見つからなかった。



参考

[Ubuntu Desktop (20.04/18.04) にWineHQをインストールする！](https://lab4ict.com/system/archives/3851)
[Ubuntu 20.04 Focal Fossa LinuxにLutrisをインストールする](https://goto-linux.com/ja/2019/8/7/ubuntu-20.04-focal-fossa-linux%E3%81%ABlutris%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B/)
[UbuntuにWine6.0をインストールする方法](https://tutorialcrawler.com/ubuntu-debian/ubuntu%E3%81%ABwine6-0%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95/)




#### 4. MTG Arena インストール


さて、ここが一番苦労したところ。


まず、Lutoris上で、MTG Arenaを探してインストールするのは簡単だった。
特に迷うことはない。
インストール場所は下記のように、自身のホーム配下にGamesというディレクトリ作って、そこにいれた。
/home/{ユーザー}/Games/magic-the-gathering-arena

手順知りたい人は下記みておくとよいかも。

[How to install Magic the Gathering: Arena on Linux with Lutris - YouTube](https://www.youtube.com/watch?v=6CeQHkl9qmU)

迷いはしなかったが、一度インストール途中に数時間動きがみられないときがあって、
僕は一度キャンセルボタン押して、止め、
再度インストールしたら、上手くいった。


で、Playボタンを押しても起動しなかった。
下記動画を見て、設定とかはなるべく合わせたのだけどだめ。

[Magic The Gathering Arena on Linux with Lutris - Wine Staging - YouTube](https://www.youtube.com/watch?v=-fQDpO7BlfU)




何がだめかというと、
MTG Arenaのロゴはでるんだけど、
Checking for Update が全く進まない。
Lutrisから、Show Logでログファイルみても、特に有用な情報が載ってない。
Wineバージョン確認したときと同様の
ERROR: ld.so ・・・(中略)
ってのは出ているんだけど、ignoredと書いてあるし、これは関係ないと判断した。


で、色々探して(一日以上探してた。。。)、Redditにそれらしいのを発見

[Lutris MTG Arena - Updating : MagicArena](https://www.reddit.com/r/MagicArena/comments/d1uhhg/lutris_mtg_arena_updating/)

最後のほうで、nguyenducminh2508 というユーザーがまとめてくれているけど、
要約すると、
* MTG Arena公式の、MSIファイルをダウンロード
* Lutoris上から、MSIをインストール
* それでインストールされたexeを、Lutorisの起動パスに設定して、起動
* この方法でしか起動できないよ

ということだ。
最初は2019年の投稿だし、半信半疑だったけど、
さりとて他にあてもないし、とりあえずやってみる。

MTG Arena公式の、MSIファイルは下記からダウンロード。
[MAR 30 - Updating MTG Arena... - Magic the Gathering: Arena](https://forums.mtgarena.com/forums/threads/58489)

hereって書いてあるところ。
この時点では最新版は2021/03/30の版
ファイル名は、
MTGAInstaller_0.1.3536.856069.msi

これをとりあえず下記に保存して、
/home/{ユーザー}/Games/MTGAInstaller_0.1.3536.856069.msi

LutrisのPlayボタン近くのワイングラスマークを押してでてきたメニューから、
Run EXE inside wine prefix
を選び、先ほどのMSIを指定すると、自動でインストールがはじまる。(開始するまでちょっとラグある)


インストール先はどこでも良さそうだが、
Wineがつかっている、Windowsを模したディレクトリ構造の中のほうがよいかと思い、
最初にMTG Arenaがインストールされた、下記を指定する。

/home/{ユーザー}/Games/magic-the-gathering-arena/drive_c/Program Files/Wizards of the Coast/MTGA


すると、インストールが進み、
終わると、上記のディレクトリに、
MTGA.exeができているので、

LutrisのPlayボタン横の三角を押してでてきたメニューから、
Configure
を選び、
Game optionsタブのExecutableを、
MTGA.exeを指し示すように書き換える。


これで後はPlayボタンを押すと、無事に起動できた！
ふー、長かった。

思うに、Lutoris上のインストールは簡易なものだけインストールして、アップデートを確認したらアップデートを自動で進めるタイプ、
MSIは、最初からアップデート不要の最新版のバージョンのインストールタイプなんだろうと思う。

ということは、MTG Arenaがアップデートしても、自動でアップデートされなさそうなので、またMSI使って入れ直さないといけないかも。
-> (2021/04/16 追記)
新しいエキスパンションのストリクスヘイヴンが出たので、
いつものように起動したらアップデートしろって言われた。
また上記の手順でMSIから入れ直し。(MTGAInstaller_0.1.3561.858192.msi になってた)
MTG Arenaは結構容量を喰うのだけど、前のを消さずに同じところにインストールしたら、
さほど容量喰わずにすんだ。
たぶんデータはそのままで、実行ファイル系だけ更新しているみたい。
アップデートしたら、普通に起動できた。


ちなみに、普通に対戦とかデッキ構築はできるけど、
ジェムの購入などは失敗したって報告もあるみたいなので、
それはiPhone版などからやるほうがいいと思う。





### 余談

MTG Arenaを始めて一週間ぐらいだけど、
メインで使ってるデッキは、
+1/+1カウンターの緑単色デッキ。
昔から緑好きだったんだよね。
神話レアのワイルドカードを使って、

[巨怪な略奪者、ヴォリンクレックス&#124;カードギャラリー&#124;マジック：ザ・ギャザリング 日本公式ウェブサイト](https://mtg-jp.com/products/card-gallery/0000201/503815/)

を作ったから、しばらくこれで遊んでみる。


あとは神話レアワイルドカードを間違えて、

[死の飢えのタイタン、クロクサ&#124;カードギャラリー&#124;マジック：ザ・ギャザリング 日本公式ウェブサイト](https://mtg-jp.com/products/card-gallery/0000186/476472/)

を作ってしまったので、
どうせならハンデスデッキでも作ろうかな。

もうすぐ新しいエキスパンションのストリクスヘイヴンも発売だから、楽しみ！








