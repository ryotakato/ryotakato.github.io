---
layout: post
title: "RustでCコンパイラ その01 環境構築"
tags : [Rust, Mac, 環境構築]
date: 2025-11-13 18:10:44
---


RustでCコンパイラを作り始めることにした。

その界隈では有名な下記の記事があって、一度挑戦したことがあるのだが、挫折したので、
再度、しかもRustでやってみる。(だからセルフコンパイルはできない)

[低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook)



なお、この記事はDockerで実装することを推奨しているが、
上記の通りにやってもMacのSiliconチップ上では動かないため、工夫が必要になる。
今回は、その内容。

なお、実装は、下記のGithubにまとめている。
Dockerfileとか、compose.ymlとかも一緒。
コンパイラの名前は、tvcc (tavi c compiler)


[ryotakato/tvcc](https://github.com/ryotakato/tvcc)



### 環境情報

まずは環境情報。
MacBookAir の、2025 M4 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		15.7.1
BuildVersion:		24G231
```


Rustはasdfでインストール済み

```bash
$ rustc --version
rustc 1.89.0 (29483883e 2025-08-04)
$ cargo version
cargo 1.89.0 (c24e10642 2025-06-23)
```

Dockerもdocker composeも、brewで入れている

```bash
$ docker version
Client:
 Version:           28.5.1
 API version:       1.51
 Go version:        go1.24.8
 Git commit:        e180ab8
 Built:             Wed Oct  8 12:16:17 2025
 OS/Arch:           darwin/arm64
 Context:           desktop-linux

Server: Docker Desktop 4.50.0 (209931)
 Engine:
  Version:          28.5.1
  API version:      1.51 (minimum version 1.24)
  Go version:       go1.24.8
  Git commit:       f8215cc
  Built:            Wed Oct  8 12:18:25 2025
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.7.27
  GitCommit:        05044ec0a9a75232cad458027ca83437aae3f4da
 runc:
  Version:          1.2.5
  GitCommit:        v1.2.5-0-g59923ef
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```


### 構成の説明


元の記事のDockerの構成は、
Linux環境下で、さらにLinuxのDockerを立てて実行するたびにコンテナを作ってクリーンな状況で実行するというもの。
この構成は同じにしたいが、
Cソースではなく、Rustソースをコンパイルして、実装していくので、
Makefileで書いてあるビルドやテストの実行はcargoなどで代用していくことになる。

さらに、MacのSiliconチップで開発するので、
普通に作っていくと、x86_64向けのアセンブラが出力されず、全く動かない。
これをどうにかしないといけない。

一応、上記のDockerにRustもインストールさせて、
ソースコードを書くことだけホストでやって、ビルド以降はすべてコンテナの中でやるという手法もあるのだが、
Rustはクロスコンパイルできるはずだと思って、その方法は取らないことにした。
進んでいてどうしても困ったら方針変えるかも。


### Rosettaインストール


まず、Apple Siliconの環境でx64_86向けのアプリケーション(ここではDockerでつくるLinuxコンテナ)を動かすため
Rosettaをいれる。
いれるのだが、僕の環境ではGoogle日本語入力いれたときに入っていた。
まあ下記でインストールできる

```bash
# インストール
$ softwareupdate --install-rosetta

# 確認
$ arch -x86_64 uname -m
x86_64
```


Rosettaの詳しい説明は下記。

[Rosetta 2 ってなに？｜M1 Mac ってなに？ ぼくにも使える？](https://zenn.dev/suzuki_hoge/books/2021-07-m1-mac-4ede8ceb81e13aef10cf/viewer/3-rosetta2)



### Rustでクロスコンパイル


次にRust

Rustのcargo buildで生成されるバイナリは、
そのままだと、Apple Siliconチップのものであり、
x86_64のLinuxコンテナ上では動かないので、
これをx86_64にコンパイルできるようにする必要がある。

Rustのクロスコンパイルは自前でもできるらしいのだが、色々調べて結構面倒なことが分かったので、
crossというクレートに頼ることにした。

[cross - crates.io: Rust Package Registry](https://crates.io/crates/cross)


バイナリだけほしいので、cargo installでグローバルにいれてしまう。

```bash
$ cargo install cross

$ cross --version
cross 0.2.5
[cross] warning: `cross` does not provide a Docker image for target aarch64-apple-darwin, specify a custom image in `Cross.toml`.
[cross] note: Falling back to `cargo` on the host.
cargo 1.89.0 (c24e10642 2025-06-23)
```


で、ソースを用意したら、cargo buildと同様にビルド
targetはx86_64で、ABIはmuslにしたが、それは下記を参考にしたから、gnuのほうがよいのかなどは正直よくわかっていない。

[📕 RustでRui Ueyama先生の低レイヤを知りたい人のためのCコンパイラ作成入門をやってみる1(環境構築から四則演算まで) &#124; Happy developing](https://blog.ymgyt.io/entry/compilerbook_with_rust_1/)


```bash
$ cross build --target x86_64-unknown-linux-musl
```


しかし、ここでエラー。
crossはビルドにdockerを使うみたいなんだけど、そのDockerイメージが落ちてこない。
色々調べて、下記をみつけた。

["no matching manifest" on cross v0.2.5 · Issue #1214 · cross-rs/cross](https://github.com/cross-rs/cross/issues/1214)

ここで、環境変数を設定していたので、やってみる。


```bash
$ CROSS_CONTAINER_OPTS="--platform linux/amd64" cross build --target x86_64-unknown-linux-musl
```


すると、すんなりコンテナが作成されて、buildが通った。
コンテナが作られていれば、2回目以降はcross build --target x86_64-unknown-linux-musl
だけでよさそう。


ちなみに、上記の参考にした日本語情報では、makeクレートも使っていたみたい。
便利そうだけどどうしようかな。
まあコマンド打つのが面倒になったら使うかも。





### Docker構成


[低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook)
では、Dockerコマンドだけなんだけど、
僕個人はたとえコンテナ1個でもdocker composeするのが好きなので、
指定されているDockerfileをローカルに落としてきて、
下記compose.ymlファイルを作っている。


```yaml
services:
  tvcc:
    container_name: tvcc
    platform: linux/amd64
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ${PWD}:/tvcc
    working_dir: /tvcc
    command: ./test.sh
```

ポイントは、platform指定をしているところ。
これにより、x86_64に対応した実行環境がビルドされ、それがコンテナになる。
後で、Docker DesktopのDashboard画面開くと、AMD64みたいな表記になっているからわかりやすい。



なお、Dockerのplatformについては下記が参考になる。

[Docker について｜M1 Mac ってなに？ ぼくにも使える？](https://zenn.dev/suzuki_hoge/books/2021-07-m1-mac-4ede8ceb81e13aef10cf/viewer/7-docker)


test.shは、こんな感じ
さっきのようにcrossコマンドでビルドすると、./targetx86_64-unknown-linux-musl/debug配下に実行ファイルtvccができるので、
それをコンテナ内での実行するファイルとしてCMD変数にパスとして書いているということ。
gccコマンド時に実行してできたアセンブリを機械語に変換して、その後実行という感じ。


```bash
#!/bin/bash

CMD=./target/x86_64-unknown-linux-musl/debug/tvcc
TARGET=./target/tmp

mkdir -p "${TARGET}"

try() {
  expected="$1"
  input="$2"

  ${CMD} "$input" > "${TARGET}/tmp.s"
  gcc -o "${TARGET}/tmp" "${TARGET}/tmp.s"
  "${TARGET}/tmp"
  actual="$?"

  if [ "$actual" = "$expected" ]; then
    echo "$input => $actual"
  else
    echo "$input => $expected expected, but got $actual"
    exit 1
  fi
}

try 0 0
try 42 42

echo OK
```



とりあえず動くようになってよかった。
そして、クロスコンパイルとか、アーキテクチャについても結構学べた。
ちなみに、Mac siliconで動くアセンブラを出力すればもっと簡単になりそうだが、そこまで手を出すと長そうなのでやめておく。

ちなみにまだ下記のようなアセンブラを出力するだけのプログラムなので、あまり意味はない。
次回からはいよいよ進めていく。

```asm
.intel_syntax noprefix
.globl main
main:
  mov rax, 42
  ret
```






### 参考


[📕 RustでRui Ueyama先生の低レイヤを知りたい人のためのCコンパイラ作成入門をやってみる1(環境構築から四則演算まで) &#124; Happy developing](https://blog.ymgyt.io/entry/compilerbook_with_rust_1/)

[[Rust] 開発環境と実行環境が違う場合のビルド方法 &#124; DevelopersIO](https://dev.classmethod.jp/articles/rust-crosscompile/)

[「低レイヤを知りたい人のためのCコンパイラ作成入門」をApple Silliconで動かす](https://zenn.dev/toririm/scraps/07f7d76e59d77d)

[hatappo/compilerbook: 『低レイヤを知りたい人のためのCコンパイラ作成入門』](https://github.com/hatappo/compilerbook?tab=readme-ov-file)

[M1 Macでchibiccを動かす - sbite’s blog](https://sbite.hatenablog.com/entry/2021/04/21/222225)

