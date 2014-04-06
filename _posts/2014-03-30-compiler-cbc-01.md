---
layout: post
title: "スパイラル方式で「ふつうのコンパイラをつくろう」 第01回"
tags : [コンパイラ, Java]
date: 2014-03-30 22:27:56
---


### 前置き

前回、書きたいことのリストを書いた癖に、
早速リスト外のことを書く。

こまけぇこたぁいいんだよ！！



### コンパイラをつくろう

以前、この本を買った。


<div class="amazlet-box" style="margin-bottom:0px;"><div class="amazlet-image" style="float:left;margin:0px 12px 1px 0px;"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4797337958/tavi06-22/ref=nosim/" name="amazletlink" target="_blank"><img src="http://ecx.images-amazon.com/images/I/419S%2BG0jL-L._SL160_.jpg" alt="ふつうのコンパイラをつくろう 言語処理系をつくりながら学ぶコンパイルと実行環境の仕組み" style="border: none;" /></a></div><div class="amazlet-info" style="line-height:120%; margin-bottom: 10px"><div class="amazlet-name" style="margin-bottom:10px;line-height:120%"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4797337958/tavi06-22/ref=nosim/" name="amazletlink" target="_blank">ふつうのコンパイラをつくろう 言語処理系をつくりながら学ぶコンパイルと実行環境の仕組み</a><div class="amazlet-powered-date" style="font-size:80%;margin-top:5px;line-height:120%">posted with <a href="http://www.amazlet.com/" title="amazlet" target="_blank">amazlet</a> at 14.03.30</div></div><div class="amazlet-detail">青木 峰郎 <br />ソフトバンククリエイティブ <br />売り上げランキング: 65,176<br /></div><div class="amazlet-sub-info" style="float: left;"><div class="amazlet-link" style="margin-top: 5px"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4797337958/tavi06-22/ref=nosim/" name="amazletlink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="amazlet-footer" style="clear: left"></div></div>



この本は、C言語からいくつか機能を取り払ったC♭ (シーフラット)という言語をJavaで作りながら、
コンパイラの作成方法を学ぼうというものだ。

買ったときに、まずはC♭ のサンプルを動かそうと思い、
何を思ったか、Macで動かそうとして失敗して、でもエラーが分からなくて、
投げて、ふてくされて、寝てしまった思い出がある。
ええ、いつもの積読です。本当にありがとうございました。


でもよくよく考えたら、サンプルを動かす必要ないんだよね。
だってそのサンプルであるC♭ をこれから作るっていう本なんだから。
なんでそのときそれに思い至らなかったんだろうね。

ってことで再び始めてみたので、とりあえずブログにつける。

なお、だんだん、いちいちC♭ って打つのが面倒になってきたから、
cbc (= C♭ Compiler) って書くよ。
あるいは、cb言語とか。




### Githubリポジトリ

まだ途中だけど、先にGithubリポジトリを晒しとく。
ソースが汚いのはご愛嬌。

[ryotakato/cbc](https://github.com/ryotakato/cbc)



### 環境

まずはある程度読み進めて、全体の流れを掴む。
この本は4部構成で、
第1部は、JavaCCを用意し、スキャナーとパーサをつくるところまで。

なので、まずはJavaCCをインストールするところから


* Mac OS X 10.9.2
* JDK 1.7.0_51 インストール済み


JavaCCは、Macportでもインストールできるんだけど、
Macportでインストールされるものは、何故かjavaccコマンドで作ったjavaファイルが実行できないんだよね。
もしかしたらバージョンの問題なのかな？と思い、

[JavaCC Home](https://javacc.java.net/)

から、直接落として、実行できるところに置いた。

[JavaCCをインストールしてみました - Qiita](http://qiita.com/t-mochizuki/items/1031862dc76b223868e2)

を参考にするといい。


でも、いちいち編集するたびに、

* javaccを実行(jjファイルから、javaファイルを作成)
* javacを実行(javaファイルから、classファイルを作成)
* javaを実行(classファイルを実行)

するのは面倒だったので、Gradleを使った。(Groovyでもないのにw)
Gradleで、cbcタスクを実行すれば、上のを全部実行してくれる。
ここは後述します。



### 字句解析から構文木作成までをとりあえず通してみる。

この本、読んでいけばわかるんだけど、進め方がウォーターフロー式なんだよね。
だから、全体の流れを掴むには、すごくいいし、
かつ、一個一個積み重ねていくから、曖昧な部分を残して進むこともない。

・・・のだけれど、
第1部だけでも、127ページまであるのに、そこまでたどり着いても動くモノができない。
この「動くものができない」は、イコール一度もjavaccコマンドを実行しないということ。
実際ちゃんと動くものができあがるのが、割と最後のほうなんだよね。それこそ第4部ぐらい。。。

要するに、ウォーターフローすぎて、退屈ってこと。
まったくコンパイラを作ったことのない素人が、ここまでモチベーションを保てるのかが疑問だと思う。


僕は結構最初のほうで退屈しだしたので、
中途半端でもいいいから、まずは何かしら動くものを作ろうと思ったわけ。
やっぱり開発方式はスパイラルが自分には合ってるのよね。



作るものとしては、ほどよい難易度がいいので、
まずは、四則演算プラスα のソースを構文木にするところまで。
実行はできなくていい。


どういうことかというと、

1 + (2 + 3)

とか、

2 * 234 - 65

とかの構文木を出力してみるということ。


予約語とか文字列とか、なにそれ状態で、
ソースコードとも呼べないものを解析するわけだけど、
やはり四則演算は理解し易いと思うので、ここから。




### 全体構成


```bash
$ tree src/main/javacc
src/main/javacc
├── ast
│   ├── AbstractAssignNode.java
│   ├── BinaryOpNode.java
│   ├── CondExprNode.java
│   ├── Dumpable.java
│   ├── Dumper.java
│   ├── ExprNode.java
│   ├── IntegerLiteralNode.java
│   ├── LiteralNode.java
│   ├── Location.java
│   ├── LogicalAndNode.java
│   ├── LogicalOrNode.java
│   ├── Node.java
│   └── OpAssignNode.java
├── exception
│   ├── FileException.java
│   └── SyntaxException.java
└── parser
    └── Parser.jj
```


字句解析、構文解析の段階で触るのは、主にJavaCCファイル(一番最後のParser.jj)


Parser.jjでは、

* 字句解析するためのトークン定義(スキャナー)
* 構文解析のためのEBNF定義(パーサー)
* 構文木作成のためのアクション定義

を記述することになる。



### スキャナー

トークン定義は、ほんと少ししかない。

```java
SPECIAL_TOKEN: { <SPACES: ([" ", "\t", "\n", "\r", "\f"])+> }
SPECIAL_TOKEN: { <LINE_COMMENT: "//" (~["\n", "\r"])* ("\n" | "\r\n" | "\r")?> }

TOKEN: {
  <INTEGER: (["0"-"9"])+>
}
```

スペースなどを表す部分と、コメント部分、そして、数値(0〜9)

これだけ


まあ、四則演算だしね。
小数とかもいれたほうが良かったかもだけど、面倒になるので、後回し。




### パーサー

パーサーは、EBNF定義を書くことで作成が可能。
全部は載せることができないので、一部だけ。
なお、下のコードは実際には動かせません(jjファイルの構文エラー)
EBNF定義の説明を単純にするための簡易コードです。

```java

primary(): 
{
  <INTEGER> | "(" expr() ")"
}

```

先ほど定義したINTEGERというトークンを使い、primaryというパーサー定義をしている。
EBNFは、INTEGER か、 括弧付きの"expr()" という構成
exprは他の部分で定義しているパーサー定義
これは、0〜9の数値か、括弧付きの式を、一つの項として抽出するための定義

これにより、

1 + ( 2 + 3 )

は、

それぞれ、"1" と "( 2 + 3 )" という項に抽出できる




### アクション

パーサー定義は、それだけでは何もできず、
これに対してアクションを定義していく必要がある。
アクションとは、そのパーサー定義に合致する部分が見つかった場合にすること

ここでは、パーサー定義に合致する部分が見つかったら、構文木を作成するためのアクション定義を行う。
さっきのprimaryに対して、アクションをつけていく

```java

ExprNode primary(): 
{
  Token t;
  ExprNode n;
}
{
  t=<INTEGER>
  {
    return integerNode(location(t), t.image);
  }
  | "(" n=expr() ")"
  {
    return n;
  }
}

```

結構変更しちゃったw 

この中で新たに出てきているのは、

* 戻り値ExprNode
* ローカル変数 t と n
* INTEGER などの左辺
* return 文

この定義全体として、一つのメソッドと考えれば分かり易いと思う。

INTEGER が見つかったら、
integerNodeメソッドを呼び出して、戻り値を返す
括弧付きの式が見つかったら、
そのまま返す
戻り値の型は ExprNode

って感じ


ExprNodeなどはあらかじめ定義しておくクラス
先ほどの全体構成のastの中に入れていたね。
integerNodeメソッドってのは、Parser.jjの中で定義しておくメソッド
まあ、ちょっとした便利メソッドの類い
Parser.jjの中には、mainメソッドなどを書いているので、合わせて書いておく。


### 動かしてみる

さて、これまでで、JavaCCファイル Parser.jj に書く内容はだいたい触れたので、
実際に動かそう。


実行にはGradleを使うって言ったので、build.gradleを晒すとこんな感じ

```groovy
apply plugin: 'java'

def defaultEncoding = 'UTF-8'

repositories {
  mavenCentral()
}

[compileJava,compileTestJava].each{it.options.encoding = defaultEncoding}

task rmDir(type: Delete) {
  delete "src/main/java"
}

task copyJava(type:Copy, dependsOn: rmDir) {
  from('src/main/javacc') {
    include '**/*.java'
  }
  into 'src/main/java'
}

task javacc(type:Exec, dependsOn: copyJava) {
  ext.srcFile = file("src/main/javacc/parser/Parser.jj")
  ext.destDir = file("src/main/java/parser")

  destDir.mkdirs()
  commandLine 'javacc', "-OUTPUT_DIRECTORY=$destDir", "$srcFile"
}

compileJava.dependsOn javacc

task cbc(type: JavaExec){
  group = "Run"
  description = "Run the cbc program"
  main = "parser.Parser"
  classpath sourceSets.main.output + sourceSets.main.runtimeClasspath
  if (project.hasProperty('args')) {
    args project.args
  }
}

```


見るべきポイントは、javaccタスクとcbcタスク。
cbcタスクを実行すると、
copyJavaタスクで、ExprNodeなど、自分で定義したJavaファイルが"src/main/java"にコピーされる
次にjavaccタスクで、javaccコマンドを使ってParser.jjをJavaファイルにしている
その後、できたJavaファイルを、cbcタスクで実行 というわけだ。

気をつけるべきは、
compileJavaの依存関係に、javaccタスクを設定しているところ

これがないと、javaccよりcompileJavaのほうが先に動いてしまい、エラーになる。




さて、では動かそう
あらかじめ、
/Users/xxxxx/cbc/sample
というファイルに、
1 + (2 + 3)
と記載しておくとして、
このファイルを解析して、構文木を出力してみると、


```bash
$ gradle -q cbc -Pargs="/Users/xxxxx/cbc/sample"
Java Compiler Compiler Version 6.0_1 (Parser Generator)
(type "javacc" with no arguments for help)
Reading from file /Users/xxxxx/cbc/src/main/javacc/parser/Parser.jj . . .
File "TokenMgrError.java" does not exist.  Will create one.
File "ParseException.java" does not exist.  Will create one.
File "Token.java" does not exist.  Will create one.
File "SimpleCharStream.java" does not exist.  Will create one.
Parser generated successfully.
<<BinaryOpNode>> (/Users/xxxxx/cbc/sample:1)
op: +
left:
    <<IntegerLiteralNode>> (/Users/xxxxx/cbc/sample:1)
    value: 1
right:
    <<BinaryOpNode>> (/Users/xxxxx/cbc/sample:1)
    op: +
    left:
        <<IntegerLiteralNode>> (/Users/xxxxx/cbc/sample:1)
        value: 2
    right:
        <<IntegerLiteralNode>> (/Users/xxxxx/cbc/sample:1)
        value: 3

```

という感じに出力される

おー、無事に出力できた！
しかもちゃんと括弧部分は内側になっている！




### まとめ


まだ簡単な解析だけだが、やっぱり動くものがあると楽しい！
ここから動かしながら肉付けしていくが、
まずは、解析した構文木を、実際にコンパイルして実行できるようにするのが、目指すところかな。
予約語とか、変数とかはあとでいい。












