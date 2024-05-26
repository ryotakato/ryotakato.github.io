---
layout: post
title: "Ubuntu 22.04LTSに、OpenJDKであるAmazon Corretto 21をインストール"
tags : [Linux, 環境構築, Java]
date: 2024-05-26 16:52:44
---

最近はどこもかしこもOpenJDKを出してて、
まるでJDK戦国時代。
Javaを15年触ってきているが、特別どこかのJDKが気に入っているということもないので、
何でも良かったが、マイクラの特別サーバを立てるために、
推奨されてた、Amazon Correttoの21で。



まずは環境情報。


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



インストール前に最新にしておく。

```bash
$ sudo apt-get update && sudo apt-get upgrade
```

そしてインストールに必要な各ツールを入れる。
ただし、僕は全部入ってた。

```bash
$ sudo apt install ca-certificates apt-transport-https gnupg wget
```




インストールのために、aptのリポジトリをこれで準備。

```bash
wget -O - https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto-keyring.gpg && \
echo "deb [signed-by=/usr/share/keyrings/corretto-keyring.gpg] https://apt.corretto.aws stable main" | sudo tee /etc/apt/sources.list.d/corretto.list
```


最新にして、

```bash
$ sudo apt update
```


インストール

```bash
$ sudo apt install -y java-21-amazon-corretto-jdk libxi6 libxtst6 libxrender1
```


入ったら、確認。

```bash
$ java -version
openjdk version "21.0.3" 2024-04-16 LTS
OpenJDK Runtime Environment Corretto-21.0.3.9.1 (build 21.0.3+9-LTS)
OpenJDK 64-Bit Server VM Corretto-21.0.3.9.1 (build 21.0.3+9-LTS, mixed mode, sharing)
```

簡単！



参考
[Ubuntu 22.04にOpenJDK 11をインストール（Amazon Corretto） #Ubuntu22.04 - Qiita](https://qiita.com/witchcraze/items/3899b0cdccb6234a57a4)

