---
layout: post
title: "GeminiとChatGPTのAPIを使って、英単語帳の画像を読み込ませてみた"
tags : [AI, 英語]
date: 2024-09-13 12:55:44
---



### きっかけ

IELTS向けにスピーキング力をつけたくて、下記を買った。

<div class="AmaQuick-box" style="margin-bottom: 0px;"><div class="AmaQuick-image" style="float: left; margin: 0px 12px 1px 0px;"><a href="https://www.amazon.co.jp/dp/B0CZ7RGMDH/?tag=tavi06-22" name="AmaQuicklink" target="_blank"><img src="https://m.media-amazon.com/images/I/51OUCAc5mIL._SL200_.jpg" alt="分野別×言い換え力　スピーキング攻略　IELTS英単語" style="border: none;"/></a></div><div class="AmaQuick-info" style="margin-bottom: 10px; line-height: 120%"><div class="AmaQuick-name" style="margin-bottom: 10px; line-height: 120%"><a href="https://www.amazon.co.jp/dp/B0CZ7RGMDH/?tag=tavi06-22" name="AmaQuicklink" target="_blank">分野別×言い換え力　スピーキング攻略　IELTS英単語</a><div class="AmaQuick-powered-date" style="font-size: 80%; margin-top: 5px; line-height: 120%">posted with <a href="https://creazy.net/amazon_quick_affiliate" title="AmaQuick" target="_blank">AmaQuick</a></div></div><div class="AmaQuick-detail">中林くみこ(著)</div><div class="AmaQuick-sub-info" style="float: left;"><div class="AmaQuick-link" style="margin-top: 5px"><a href="https://www.amazon.co.jp/dp/B0CZ7RGMDH/?tag=tavi06-22" name="AmaQuicklink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="AmaQuick-footer" style="clear: left"></div></div>


そしたら、著者が、下記のようにAIを使って単語帳の画像から中身を読み取る方法を示していたので、
こっちでもやってみようと思った。


<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">GoogleのAI「Gemini」を使うと、紙の単語帳からアプリのフラッシュカードが簡単に作れます。面倒な単語の入力作業が不要なので、かなり時短になります。<br><br>1. 単語帳をカメラで撮る<br>2. Geminiに表形式で抜き出してもらう<br>3. スプレッドシートにエクスポート<br>4. 暗記アプリ指定のフォーマットに整える… <a href="https://t.co/jPRdkhQndD">pic.twitter.com/jPRdkhQndD</a></p>&mdash; Kumiko｜英語学習を高速化 (@IELTS_expert) <a href="https://twitter.com/IELTS_expert/status/1803703451551146188?ref_src=twsrc%5Etfw">June 20, 2024</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


僕個人としては、単語自体は別にいらないんだけど、
例文と例文の日本語訳を、愛用しているQuizletに読み込ませて、
英作文をしながら単語も一気に覚えていきたいと思った。

AIは今まで使っていなかったので、その勉強にもなるし。



### 手順

手順としては大きく分けて下記2つ


1. 電子書籍の単語帳を全部スクリーンショット
2. 画像をAIで読み込んでファイル化 





### 電子書籍のスクリーンショット撮影


iPhoneにKindleいれていたので、そこでスクリーンショットを撮影していたのだが、
1枚1枚やるのは面倒。全部で100ページあるし。

ということで、下記を参考に一定秒数でスクリーンショットを取り続けるiOSのショートカットを作ってみました。
[【ショートカット】連続してスクリーンショットを撮影するショートカット &#124; よっしー@ゆるミニマリスト](https://note.com/44woker/n/n578310b8aa0a)


あ、Kindleをスライドしてページめくりするのは手動でやるしかないです。


まずは起動用のショートカット
最初5秒待つようにしているのは、その間にアプリを切り替えてKindleにするため。

![compare-ai-via-image-ocr_01.jpeg]({{ BASE_PATH }}/images/2024/09/13/compare-ai-via-image-ocr_01.jpeg)


それとは別に1秒間隔でスクリーンショットを取り続けるショートカット

![compare-ai-via-image-ocr_02.jpeg]({{ BASE_PATH }}/images/2024/09/13/compare-ai-via-image-ocr_02.jpeg)

![compare-ai-via-image-ocr_03.jpeg]({{ BASE_PATH }}/images/2024/09/13/compare-ai-via-image-ocr_03.jpeg)

iOSのショートカットは、通常のループだと回数を指定しないといけない。
それだと面倒なので、
無限ループするように、自分を再帰的に呼び出すようにした。

で、そこで引数で何回撮影したかを渡していき、
10回撮影したら、まだ続けるかを聞くようにしている。
ここでキャンセルを押せば、ループから抜けることができるようにした。

ちなみに、1秒待機を入れているので、万が一判定がミスって無限ループに落ちいったとしても
その間にiPhoneを操作して再度ショートカットのアプリに戻ってきて、起動しているショートカットを止めればよい。



ということで、
何度かミスったけど、
最終的に、無事に単語帳の期待したページを100枚撮影することができた。




### AIで読み取る Gemini編


まずは環境情報。
MacBookAir の、2023 M2 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		14.6.1
BuildVersion:		23G93
```



著者が言っていたように、まずはGeminiから試してみた。
最初はブラウザからGeminiにアクセスして、1枚画像アップロードして、抜き出すように試していたんだけど、
できそうな感じだと思ってからは、APIKeyを作成して、Pythonでアクセスしてみた。

ソースコードは下記。
Githubにあげるほどでもなかったので、ベタで貼る。
APIKeyは流石に隠したけど。

```python

import pathlib
import textwrap
import os
import glob

import google.generativeai as genai

from PIL import Image


def main(args: argparse.Namespace):

    # 環境変数に GOOGLE_API_KEY が設定されていれば不要
    # GOOGLE_API_KEY=os.getenv('GOOGLE_API_KEY')
    #genai.configure(api_key="GOOGLE_API_KEY")
    genai.configure(api_key="xxxxxxxxxxxxxxx")


    gemini_pro = genai.GenerativeModel("gemini-1.5-pro")


    # 複数ファイル一気に読み取りのためにinputディレクトリ内にジャンル別で分けたディレクトリを指定
    genre = args.genre


    # プロンプトは試行錯誤
    #prompt = 'これらの画像から、"例文の日本語訳"と"例文"の項目だけをOCRで読み取って、表にして'
    #prompt = 'これらの画像から、"例文の日本語訳"と"例文"の項目だけを抜き出して、カンマ区切りにして'
    prompt = 'これらの画像から、"例文の日本語訳"と"例文"の項目だけを抜き出して、カンマ区切りの表にして'
    #prompt = 'これらの画像から、下記項目の文字をOCRで読み取って、表にして'\
        #         '・例文の日本語訳'\
        #         '・例文'


    # 画像ファイルのPATH取得
    files = sorted(glob.glob(f'./input/{genre}/*.jpeg'))

    # 画像取り出し
    images = []
    for f in files:
      img = Image.open(f)
      images.append(img)

    # 出力ディレクトリがなかったら作成
    make_dir_path = f'./output/{genre}'
    if os.path.isdir(make_dir_path):
      pass
    else:
      os.makedirs(make_dir_path)

    # GeminiにAPIを投げる
    response = gemini_pro.generate_content([prompt, *images])

    # 結果をファイルに追記
    with open(f'./output/{genre}/output.txt', 'a') as of:
      of.write(response.text)

if __name__ == '__main__':
    
    parser = argparse.ArgumentParser()
    parser.add_argument('genre', type=str, help='What genre')
    args = parser.parse_args()

    main(args)



```

指定している通り、gemini-1.5-proのモデルを使った。
量が多いと制限に引っかかるかなと思ったけど、100枚の画像程度なら引っかからずにいけた。

outputされたファイルを確認して、整形などをやっていたら、
日本語が明らかにおかしいところをちらほら発見。
それだけならまだいいんだけど、別々の例文が1つにまとまってしまっているところもあった。
そして例文の英語も微妙に差し替えられているところも。

気づいて直していったけど、822個の例文と日本語訳を目検で直すのはキツいな。

無料枠だし、まだまだ現時点だとここが限界かなーって思っていたが、
一応有名なChatGPTも試すことに。



### AIで読み取る ChatGPT編

同様にChatGPTでもブラウザで試してから、APIで実験
ChatGPTは、systemという、そのAIの挙動を制御するための設定があるらしい。
ここでは、OCRに徹してもらうために、余計なことすんな（意訳）ということを書いておいた。

また、Geminiでは複数画像を一気に送ったが、こっちでは1画像ごとに1API投げている。
なぜなら、ChatGPTのAPIを試しているときに、
たまに “I’m sorry, I can’t provide help with that request” などのメッセージを返してきて、
結果が得られないということがあったから。
しかも同じ画像送っても起きるときと起きないときがある。
これはGeminiでは起きなかった点。

ただし、これはモデルの指定がgpt-4o-2024-08-06だからかもしれない。
値段が高いので最初は敬遠していたが、gpt-4oのほうは一度もエラーを返さなかった。


以下ソースコード


```python


import pathlib
import textwrap
import os
import glob

import time

from PIL import Image

from openai import OpenAI
import base64
import argparse



def encode_image(image_path):
  with open(image_path, 'rb') as image_file:
    return base64.b64encode(image_file.read()).decode('utf-8')



def main(args: argparse.Namespace):

    # 環境変数に OPEN_API_KEY が設定されていれば不要
    # OPENAI_API_KEY=os.getenv('OPENAI_API_KEY')
    #client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))
    client = OpenAI(api_key="xxxxxxxxxxxxxxx")
    
    # 複数ファイル一気に読み取りのためにinputディレクトリ内にジャンル別で分けたディレクトリを指定
    genre = args.genre

    # 画像ファイルのPATH取得
    files = sorted(glob.glob(f'./input/{genre}/*.jpeg'))
    
    # 出力ディレクトリがなかったら作成
    make_dir_path = f'./output/{genre}'
    if os.path.isdir(make_dir_path):
      pass
    else:
      os.makedirs(make_dir_path)


    
    for f in files:
        base64_image = encode_image(f)


        user_content = [{
            "type": "text",
            "text": 'この画像から"例文"と"例文の日本語訳"の項目を抜き出して、"例文の日本語訳"のほうだけスペースを取り除いてから、タブ区切りの表にして'
            }]
        user_content.append({
            "type": "image_url",
            "image_url": {
                "url": f"data:image/jpeg;base64,{base64_image}",
                }
            })

        system_block = {
            "role": "system",
            "content": "You are an Optical Character Recognition (OCR) machine. You will extract all the characters from the image file provided by the user, and you will only privide the extracted text in your response. As an OCR machine, You can only respond with the extracted text.The response should be output in CSV format."
            }

        user_block = {
            "role": "user",
            "content": user_content
            }
        

        messages = [
            system_block,
            user_block
        ]
        
        #print(messages)

        # OpenAI APIを呼び出してレスポンスを取得
        res = client.chat.completions.create(
            #model="gpt-4o-2024-08-06",  # 使用するモデル
            model="gpt-4o",  # 使用するモデル
            messages=messages,
            temperature=0.0,
        )

        # API実行状況と使用トークン数をチェックするためprint
        print(f)
        print(res.usage)
        
        # 結果をファイルに追記
        with open(f'./output/{genre}/output.txt', 'a') as of:
          of.write(f)
          of.write("\n")
          of.write(res.choices[0].message.content)
          of.write("\n")
          of.write("\n")

        # 連続送信しないように待つ(けど、あまり意味ないかも
        time.sleep(3)

if __name__ == '__main__':
    
    parser = argparse.ArgumentParser()
    parser.add_argument('genre', type=str, help='What genre')
    args = parser.parse_args()

    main(args)



```

ということで、エラーが返ってきた画像は再実行という感じで実行していったら、
最終的に822単語分の例文が得られた。

こちらはGeminiと違って、英文の間違いは今のところ1個も見つかっていない。
英文が混ざっていることもない。

日本語訳は微妙に意訳があるが、
それでも全体としての意味を崩すものではないので、
厳密には単語帳の日本語訳と同じになっていないけど、まあいいかなって思った。


なお、ChatGPTのAPIを使うためには、課金が必要。
以前は無料枠があったみたいだけど、2024/09/08現在はすでに廃止されており、
使う際に最低5ドル(USD)を課金しないといけなかった。

なお、1回のトークン数1500ぐらいのAPIリクエストを、失敗も含めて200回ぐらい投げたけど、
全部でまだ1ドル分ぐらいしか使っていないので、大きな画像でなければそこまで心配する必要もなさそう。



### まとめ

結果として、ChatGPTのほうが期待結果が得られた。
これは当然GeminiよりChatGPTのほうが先行しているからと考えることもできるが、
ChatGPTのsystemで指定したように、そのAIの動きを制御するところの有無だと思う。
Geminiも探せばあるのかもしれない。

さて、AIを使って楽になったかというと、
それはもちろんそうだが、
AIを効率よく使うために色々と工夫しないといけなくて、
yak shaving状態に陥りそうだった。
まだまだ完全にAI任せではダメだなと思った。


さて、得られたファイルはQuizletに読み込んで英語の勉強していこう。
ここはAIに任せるわけにはいかないもんなー（笑）





参考

Gemini API

[Gemini API 触ってみる](https://zenn.dev/open8/articles/tried-using-gemini)


ChatGPT API

[ChatGPT APIの使い方【Python編 &#124; ソースコードあり】](https://zenn.dev/umi_mori/books/chatbot-chatgpt/viewer/openai_chatgpt_api_python)
[OpenAIのAPI使って簡単な画像認識を実装してみた【Python】 &#124; yuki@AIヒロイン研究P](https://note.com/yuki_tech/n/n3bd7a7d3cd08)
[画像認識が向上した ChatGPT-4o で名刺を読み取る #Python - Qiita](https://qiita.com/kanekoyuichi/items/f3c993ae5d4ceaf21de2)




