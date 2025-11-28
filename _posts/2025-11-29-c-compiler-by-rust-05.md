---
layout: post
title: "RustでCコンパイラ その05 STEP12まで(if else, for, whileの制御文)"
tags : [Rust]
date: 2025-11-29 11:01:44
---


下記をRustで実装する続き。
[低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook)


Githubは下記。
commitコメントに、learnt step N って書いているから、該当のところをみれば変更が分かると思う。

[ryotakato/tvcc](https://github.com/ryotakato/tvcc)


ifはそこまで苦労しなかった。
だけど、Blockや空行がまだ実装していないので、ワンライナーのifとかelseしか書けない。


しかし、whileやforが上手く行かず、
かつアセンブリの量も多くなるので、どうして上手くいかないのかがアセンブリを見てもわからない。

なので、比較のために、実際のc言語の結果を逆アセンブルしてみるようにした。

例えば下記のような簡単なC言語 (戻り値は10になる)は、

```c
int main() {
  int i = 0;
  while (i<10) i=i+1; 
  return i;
}
```

下記のようにobjdumpで逆アセンブルして、main関数だけ表示させることができる。


```bash
$ objdump -d -M intel --disassemble=main test1

test1:     file format elf64-x86-64


Disassembly of section .init:

Disassembly of section .plt:

Disassembly of section .plt.got:

Disassembly of section .text:

0000000000001129 <main>:
    1129:       f3 0f 1e fa             endbr64
    112d:       55                      push   rbp
    112e:       48 89 e5                mov    rbp,rsp
    1131:       c7 45 fc 00 00 00 00    mov    DWORD PTR [rbp-0x4],0x0
    1138:       eb 04                   jmp    113e <main+0x15>
    113a:       83 45 fc 01             add    DWORD PTR [rbp-0x4],0x1
    113e:       83 7d fc 09             cmp    DWORD PTR [rbp-0x4],0x9
    1142:       7e f6                   jle    113a <main+0x11>
    1144:       8b 45 fc                mov    eax,DWORD PTR [rbp-0x4]
    1147:       5d                      pop    rbp
    1148:       c3                      ret

Disassembly of section .fini:

```

最初は自分で作っているアセンブリと全然違うやん！って思ったけど、
よく読むとなんとなく分かってくる。
jmpやjle命令で制御していて、その先は具体的なアドレス位置であり、
自分で作っているようにわざわざブロックを作ったりはしていないみたい。
また、値はポインター？なのかPTRというところに入れているらしい。
しかも数値は0x1とか0x9とかの16進数。
また、raxレジスタは64bitだからか使っておらず、eaxレジスタになっている。

とまあ、違いはあるにせよ。そう大きな違いではないので、
自分が作っているものが、実際のC言語に近いものになっていくことが知れて嬉しかった。
なお、これを参考にしても自分のwhileとforが上手く動かない理由は分からなかったけど（笑）


なので、次にgdbのデバッガを使ってみた。
(tmpがプログラム名)

```bash
# ファイルを指定してデバッガ起動
$ gdb tmp

# intel形式に変更
(gdb) set disassembly-flavor intel

(gdb) disass
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     push   rbp
   0x000000000000112a <+1>:     mov    rbp,rsp
   0x000000000000112d <+4>:     sub    rsp,0xd0
   0x0000000000001134 <+11>:    mov    rax,rbp
   0x0000000000001137 <+14>:    sub    rax,0x8
   0x000000000000113b <+18>:    push   rax
   0x000000000000113c <+19>:    push   0x0
   0x000000000000113e <+21>:    pop    rdi
   0x000000000000113f <+22>:    pop    rax
   0x0000000000001140 <+23>:    mov    QWORD PTR [rax],rdi
   0x0000000000001143 <+26>:    push   rdi
   0x0000000000001144 <+27>:    pop    rax
   0x0000000000001145 <+28>:    mov    rax,rbp
   0x0000000000001148 <+31>:    sub    rax,0x8
   0x000000000000114c <+35>:    push   rax
   0x000000000000114d <+36>:    pop    rax
   0x000000000000114e <+37>:    mov    rax,QWORD PTR [rax]
   0x0000000000001151 <+40>:    push   rax
   0x0000000000001152 <+41>:    push   0xa
   0x0000000000001154 <+43>:    pop    rdi
   0x0000000000001155 <+44>:    pop    rax
   0x0000000000001156 <+45>:    cmp    rax,rdi
   0x0000000000001159 <+48>:    setl   al
   0x000000000000115c <+51>:    movzx  rax,al
   0x0000000000001160 <+55>:    push   rax
   0x0000000000001161 <+56>:    pop    rax
   0x0000000000001162 <+57>:    cmp    rax,0x0
   0x0000000000001166 <+61>:    je     0x118d <main+100>
   0x0000000000001168 <+63>:    mov    rax,rbp
   0x000000000000116b <+66>:    sub    rax,0x8
   0x000000000000116f <+70>:    push   rax
   0x0000000000001170 <+71>:    mov    rax,rbp
   0x0000000000001173 <+74>:    sub    rax,0x8
   0x0000000000001177 <+78>:    push   rax
   0x0000000000001178 <+79>:    pop    rax
   0x0000000000001179 <+80>:    mov    rax,QWORD PTR [rax]
   0x000000000000117c <+83>:    push   rax
   0x000000000000117d <+84>:    push   0x1
   0x000000000000117f <+86>:    pop    rdi
   0x0000000000001180 <+87>:    pop    rax
   0x0000000000001181 <+88>:    add    rax,rdi
   0x0000000000001184 <+91>:    push   rax
   0x0000000000001185 <+92>:    pop    rdi
   0x0000000000001186 <+93>:    pop    rax
   0x0000000000001187 <+94>:    mov    QWORD PTR [rax],rdi
   0x000000000000118a <+97>:    push   rdi
   0x000000000000118b <+98>:    je     0x1145 <main+28>
   0x000000000000118d <+100>:   pop    rax
   0x000000000000118e <+101>:   mov    rax,rbp
   0x0000000000001191 <+104>:   sub    rax,0x8
   0x0000000000001195 <+108>:   push   rax
   0x0000000000001196 <+109>:   pop    rax
   0x0000000000001197 <+110>:   mov    rax,QWORD PTR [rax]
   0x000000000000119a <+113>:   push   rax
   0x000000000000119b <+114>:   pop    rax
   0x000000000000119c <+115>:   mov    rsp,rbp
   0x000000000000119f <+118>:   pop    rbp
   0x00000000000011a0 <+119>:   ret
   0x00000000000011a1 <+120>:   pop    rax
   0x00000000000011a2 <+121>:   mov    rsp,rbp
   0x00000000000011a5 <+124>:   pop    rbp
   0x00000000000011a6 <+125>:   ret
```


ああ、こっちのほうが全然見やすい。



のだが、runで起動しても上手く進まずに落ちてしまう。

調べてみたら、下記がみつかった。

[Input/output error message when running programs in GDB - Stack Overflow](https://stackoverflow.com/questions/79796540/input-output-error-message-when-running-programs-in-gdb)
[Unable to debug amd64 binaries on apple silicon · Issue #6921 · docker/for-mac](https://github.com/docker/for-mac/issues/6921)

そのため、Docker起動のためのcompose.ymlに、cap_addとsecurity_optを追加する

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
    cap_add: 
      - SYS_PTRACE
    security_opt: 
      - seccomp=unconfined

```

で、下記のようなmygdb.shを用意。
上記githubのissueの途中ぐらいにあるbashスクリプトを少し改造した感じ。
最後の2つ。でもここは便利なように変えていくかも。

```bash

#!/bin/bash

SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )

if [ "$#" != "1" ]; then
    echo "Usage: $0 <path/to/program to debug>" >&2
    exit 1
fi

prog="$1"

# Start program in background
ROSETTA_DEBUGSERVER_PORT=1234 "$prog" &

# Run real gdb and tell it to attach
/usr/bin/gdb \
    -iex "set architecture i386:x86-64" \
    -iex "file $prog" \
    -iex "target remote localhost:1234" \
    -iex "set history save on" \
    -iex "set disassembly-flavor intel" \
    -iex 'disp/i $pc'

```

で、./mygdb.sh ./test1 のように対象プログラムを引数にして動かせば、
あとは、b mainとかでブレイクポイントおいて、
cで実行を開始(runは効かない)
info registersとか、display $raxとかで値を確認しながらsiでステップ実行していく。


なお、gdbの使い方は下記を参考にした。
[gdb超入門 #入門 - Qiita](https://qiita.com/miyagaw61/items/4a4514e2de0b458c2589)
[GDBの使い方メモ – GitHub 出張所 – プログラム関係のブログはここに](https://nkon.github.io/Gdb-basic/)




そんなこんなでデバッグしていると、
ループのためのjmp命令が、 je命令になっていて、いつでもスルーされており、
ループ内が一回しか実行されていないことが分かった。

je命令をjmpになおして実行してみると上手くいった。



デバッグの方法覚えたのはかなりの収穫だな。

次はSTEP 13のブロック







