---
layout: post
title: "Dockerでのホストとコンテナの権限問題について"
tags : [Docker, Linux]
date: 2020-08-10 21:47:44
---


Dockerおよびdocker-composeでReact.jsの開発環境を準備しようと思ったのだが、
ホストとコンテナの権限ではまったので、備忘として記しておく。


なお、コンテナとしては、
Docker Hubのnodeイメージを元に、
コンテナを作るときにcreate-react-appにてプロジェクトを用意する。




### 環境

* Ubuntu 18.04 LTS
* Docker 19.03.12
* docker-compose 1.17.1



### コンテナ準備

まずはDockerfile
元にするイメージはnodeの最新LTSのslimバージョン
最初はalpineのサイズが小さいのが魅力的だったが、
bashじゃなかったりして、のちのちPaaSにあげたときとかに問題になりそうだったので、slimにした。

Dockerfile_reactというファイル名にする。

```
FROM node:12.18.3-slim
WORKDIR /usr/src/app
```

次にdocker-compose.yml
特に変わったところはなし。
コンテナ上では/usr/src/appで開発し、
それはホストの./frontendにマッピングする。

```yml
version: '3'
services:
  frontend:
    build:
      context: .
      dockerfile: Dockerfile_react
    volumes:
     - ./frontend:/usr/src/app
    command: sh -c "cd mern-todo && yarn start"
    ports:
     - "3000:3000"
    tty: true
```

実際にビルドとコンテナ立ち上げ

```bash
# Dockerfileとdocker-compose.ymlのbuildに則ってビルド
$ docker-compose build
# コンテナを立ち上げて、コマンドを実行し、コンテナを終了
$ docker-compose run --rm frontend sh -c "npm install -g create-react-app && create-react-app mern-todo"
# コンテナを立ち上げ
$ docker-compose up -d
```





### 問題点

コンテナは通常、ユーザーはuid=0つまりrootで動く。
そのため、立ち上げたコンテナ上で動くプロセスはrootになり、
そこで作られるファイルやディレクトリはownerがrootになってしまう。
これだけなら問題はないが、
さらにそれらのファイルがボリュームにより、ホストと共有していると、
ホスト上でもrootがownerとなってしまう。

それにより、ホストの一般ユーザーのディレクトリの下にrootのファイルが存在し、
ホスト上で編集できなくなってしまう。






### うまくいかなかった方法(コンテナ内の一般ユーザーで作業する)

下記のリンクで紹介されている方法。


[dockerでvolumeをマウントしたときのファイルのowner問題 - Qiita](https://qiita.com/yohm/items/047b2e68d008ebb0f001)
[Docker Composeのvolumes使用時に出会うpermission deniedに対応する一つの方法 - Qiita](https://qiita.com/cheekykorkind/items/ba912b62d1f59ea1b41e)



一般ユーザーを作ることはできたので、
上記のリンクが間違っているわけではないが。
今回のケースでは、
コンテナを作ったあとにnpmなどを利用する関係で、rootじゃないと権限エラーになってしまう。
それに、コンテナという限られた環境で、何故rootじゃだめなのかという気持ちもあるので、
なるべくrootでいきたかった。



### うまくいった方法(userns-remap機能を使う)

Linuxには、userns-remapという機能があり、
これは、Dockerのようなコンテナ技術と関係が深い機能だと思うが、
ホストとコンテナで、ユーザーの対応をコントロールすることができる。
今回はこれを使い、ホストの一般ユーザーとコンテナのrootを結びつけることにした。


[ユーザー名前空間でDockerコンテナのrootとホストのユーザをマッピングする &#124; Unskilled?](https://unskilled.site/%E3%83%A6%E3%83%BC%E3%82%B6%E3%83%BC%E5%90%8D%E5%89%8D%E7%A9%BA%E9%96%93%E3%81%A7docker%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E3%81%AEroot%E3%81%A8%E3%83%9B%E3%82%B9%E3%83%88%E3%81%AE%E3%83%A6%E3%83%BC/)
[宇宙の晴れ上がり: Ubuntu 18.04のDockerでUser remapを使う](http://transparent-to-radiation.blogspot.com/2018/06/ubuntu-1804dockeruser-remap.html)


上記で紹介されている通りに試してみる。

ちなみに、Ubuntuでは、
あらかじめユーザ名前空間は有効化されている。


まずは、/etc/subuidと/etc/subgidファイルに下記を追記する。
dockremapは、Docker用のデフォルト設定名。
最初の数字はコンテナのrootを何番のUIDを持つホストユーザーに割り当てるかを示す。
ここでは、1000つまりホストの一般ユーザーに割り当てている。
そこから、後ろの数字まで、ユーザーマッピングが続く。


```
// subuid
dockremap:1000:65536
// subgid
dockremap:1000:65536

```

次に、/etc/docker/daemon.json
このファイルは存在しなかったので、新しく作った。


```
{
  "userns-remap": "default"
}
```


ここまでやったら、
Dockerデーモンを再起動。

```bash
sudo systemctl restart docker
```



そして、最初に記載したコンテナの準備手順を実行すると、
ホストからは一般ユーザー、
コンテナからはrootで扱うことができるようになる。


なお、やり直す前には、
docker rm や
docker rmi などで、
不要なコンテナやイメージをけしておくと良い。




### まとめ

Dockerはかなり心理的安全性が高く、
かつ、気軽に作って壊してができるので、非常に楽。
ただし、今回のようにはまったときは、Linuxの知識が必要なので、
結構大変。






