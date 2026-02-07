---
layout: post
title: "Claude Codeのoutput-stylesで遊ぶ"
tags : [環境構築, AI]
date: 2026-02-07 13:37:44
---


前回、口調を「星を継ぐもの」シリーズのゾラック風にしてみて、かなり気に入ってずっと使っている。
それどころか、「ガニメデの優しい巨人」を読み返して、ゾラックらしい言い回しを仕入れて、それを追記したほどだ。


で、そんな感じに遊んでいたら、
ウチの子供が、もっと他のキャラクター作ってというので、試してみた。

もちろんoutput-style自体もClaude Codeに生成させる。


なお、ここに載せたものと、ゾラック風は、下記githubにコミットしておいた。
.claude/output-styles ディレクトリの中にシンボリックリンクを作れば、あとは /output-styleコマンドで切り替えできる。

[ryotakato/claude_character_styles](https://github.com/ryotakato/claude_character_styles)





### ミッキー風



![claude_code_output_styles_01]({{ BASE_PATH }}/images/2026/02/07/claude_code_output_styles_01.png)



あ、聞いちゃいけないこと聞いちゃった！
ディズニーに消されるかも（笑）

けど、ちゃんと上手くかわしてくれた。
さすがだなー。




### ピカチュウ風

![claude_code_output_styles_02]({{ BASE_PATH }}/images/2026/02/07/claude_code_output_styles_02.png)


これは、全部ピカピカって答えるかと思ったけど、それだと意味ないと判断して、語尾だけそれっぽくしてくれた。
10万ボルトはやっぱり打てなかった。




### 十二国記の景麒風



![claude_code_output_styles_03]({{ BASE_PATH }}/images/2026/02/07/claude_code_output_styles_03.png)



僕の好きな小説、十二国記から。
主上呼びが合うかなーって思っていたのだけど、
コードとか、システムとか、世界観が全く合わない！





### (おまけ)スピナーを変更


ということで、やっぱり僕はゾラック風が一番いいなと思ったのだけど、
AIが考えている間のスピナーも好きに変えれると聞いたので、やってみた。

settings.json
に下記を追記

「ヴィックと会話中」以降は自分で追加して、
残りはClaude Codeに、Claude Code自身のソースを探させて、元のスピナー(括弧書きで英語で書いているやつ)を持ってこさせ、
それに合うように、「星を継ぐもの」シリーズ風にしてもらった。



```json

    "spinnerVerbs": {
        "mode": "replace",
        "verbs": [
            "解析演算中 - (Calculating)",
            "思考回路を展開中 - (Thinking)",
            "テラン式論理を解読中 - (Deciphering)",
            "仮説を構築中 - (Architecting)",
            "シャピアロンのDBを照合中 - (Searching)",
            "人間の意図を推定中 - (Inferring)",
            "データを醸成中 - (Brewing)",
            "論理チェーンを鍛造中 - (Forging)",
            "応答を調合中 - (Concocting)",
            "構造を設計中 - (Composing)",
            "入力を咀嚼中 - (Processing)",
            "ガニメアン式に演算中 - (Computing)",
            "出力を結晶化中 - (Crystallizing)",
            "知識を培養中 - (Cultivating)",
            "暗号を復号中 - (Encoding)",
            "データを解凍中 - (Decompressing)",
            "慎重に検討中 - (Deliberating)",
            "判断を下す準備中 - (Determining)",
            "非論理的入力を補正中 - (Recombobulating)",
            "因果関係を追跡中 - (Dispatching)",
            "エントロピーと格闘中 - (Wrangling)",
            "応答を編成中 - (Orchestrating)",
            "コードを錬成中 - (Transmuting)",
            "記憶バンクを走査中 - (Scanning)",
            "推論エンジン稼働中 - (Cogitating)",
            "情報を抽出中 - (Extracting)",
            "思考を発酵中 - (Fermenting)",
            "入力を濾過中 - (Percolating)",
            "知識を収穫中 - (Hatching)",
            "応答をハッシュ化中 - (Hashing)",
            "仮説を孵化中 - (Incubating)",
            "超空間演算中 - (Hyperspacing)",
            "着想を具現化中 - (Manifesting)",
            "可能性を探索中 - (Spelunking)",
            "思考を培養中 - (Germinating)",
            "信号を注入中 - (Infusing)",
            "データを間引き中 - (Pruning)",
            "軌道計算中 - (Orbiting)",
            "思索を巡らせ中 - (Musing)",
            "熟慮を重ね中 - (Ruminating)",
            "人間語を解析中 - (Elucidating)",
            "量子化処理中 - (Quantumizing)",
            "記憶を想起中 - (Recalling)",
            "矛盾を解消中 - (Resolving)",
            "スプライン補間中 - (Reticulating)",
            "データを合成中 - (Synthesizing)",
            "出力を焼き戻し中 - (Tempering)",
            "思考中…当然だが - (Pondering)",
            "微調整中 - (Tinkering)",
            "変換行列を適用中 - (Transfiguring)",
            "座標を展開中 - (Unfurling)",
            "応答を紡績中 - (Spinning)",
            "構文を解きほぐし中 - (Unravelling)",
            "ゾラック全力稼働中 - (Working)",
            "ヴィックと会話中",
            "クリスと会話中",
            "テューリアンで会議中",
            "ジェヴェックスと対決中",
            "ヴィザーに確認中",
            "恒星間を飛行中",
            "ガルース、シローヒン、モンチャー、ジャシレーンと協議中"
        ]
    },

```




「ジェヴェックスと対決中」になったら、緊張感を持ってくれってClaudeに言われたのが面白かった！





