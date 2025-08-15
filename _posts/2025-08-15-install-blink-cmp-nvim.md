---
layout: post
title: "NeoVimの補完としてblink.cmpをインストール"
tags : [Mac, 環境構築, Vim]
date: 2025-08-15 12:12:05
---


[NeoVimを見直して、Lua全面採用してみた。](/2025/08/07/reconsider-neovim)

上記でNeoVim環境を見直してみたが、補完系プラグインについてちょっと別個で切り出して書いてみる。

なお、設定はいつものように dotfilesとしてgithubにあげてある。

[ryotakato/dotfiles](https://github.com/ryotakato/dotfiles)




### 補完系プラグインはどれがよいか？


AI全盛なこの時代、もはやAI補完でやるのが当たり前なのかもしれないが、
そこまで大げさなものを入れたくなく、最近書いた内容だったり、同じファイル内にある変数名だったりを補完してくれれば十分である僕にとって、
どういうプラグインがよいのか？

NeoVimで補完といえば、
[hrsh7th/nvim-cmp: A completion plugin for neovim coded in Lua.](https://github.com/hrsh7th/nvim-cmp)
が、人気があって多くの人が使っているのだと思うが、
作者が下記を書いていて、
[ヘッドレスな補完エンジンをつくりたい話](https://zenn.dev/hrsh7th/articles/1d558a56084fe5)
別途補完プラグインを作っている最中とのこと。
そうなると、遅かれ早かれnvim-cmpが非推奨になったりしないかなという不安があり、採用は見送る。
実際そこまで心配しなくていいのかもだけど、
めっちゃ活用するわけではない僕の使い方だと、使えなくなって乗り換える手間が非常に嫌。



また、カスタマイズ性を重視するなら、Shougoさんの系統である、
[Shougo/ddc.vim: Dark deno-powered completion framework for Vim/Neovim](https://github.com/Shougo/ddc.vim)
なんだけど、
一度その系統から離れた身としては、また使うのは微妙。
それにdeno依存もあるしね。（それ自体にほとんど影響ないとはいえ）



で、新し目のとして、
[Saghen/blink.cmp: Performant, batteries-included completion plugin for Neovim](https://github.com/Saghen/blink.cmp)

というのが見つかり、設定もそう難しくはなさそうということで使ってみることにした。





### blink.cmpをlazy.nvimでインストール


以下にlazy.nvimでのluaファイルを書いておきます。
設定は色々と考えてこうなっています。




```lua
return {
  'saghen/blink.cmp', 
  version = "*",
  event = { "InsertEnter", "CmdLineEnter" },
  ---@module 'blink.cmp'
  ---@type blink.cmp.Config
  opts = {
    keymap = {
      preset = "enter",
      -- change TAB, S-TAB 
      ['<Tab>'] = {
        function(cmp)
          if cmp.snippet_active() then return cmp.accept()
          else 
            local list = require('blink.cmp.completion.list')
            if #list.items == 1 then
              return cmp.select_and_accept()
            else
              return cmp.select_next()
            end
          end
        end,
        'snippet_forward',
        'fallback'
      },
      ['<S-Tab>'] = {
        function(cmp)
          if cmp.snippet_active() then return cmp.accept()
          else 
            local list = require('blink.cmp.completion.list')
            if #list.items == 1 then
              return cmp.select_and_accept()
            else
              return cmp.select_prev()
            end
          end
        end,
        'snippet_backward',
        'fallback'
      },
    },
    cmdline = {
      keymap = {
        preset = "enter",
      },
      completion = { menu = { auto_show = true } },
    },
    sources = {
      default = { "snippets", "lsp", "path", "buffer", "cmdline" },

      min_keyword_length = function(ctx)
        -- :wq, :qa -> menu doesn't popup
        -- :Lazy, :wqa -> menu popup
        if ctx.mode == "cmdline" and ctx.line:find("^%l+$") ~= nil then
          return 3
        end
        return 0
      end,
    },
    appearance = {
      nerd_font_variant = "mono",
    },
    -- border
    completion = {
      documentation = { window = { border = "rounded" } },
      menu = { border = "rounded" },
      list = { selection = { preselect = false } }, -- auto select false
    },
    signature = { window = { border = "rounded" } },

    fuzzy = {
      implementation = "prefer_rust_with_warning",
    },
    opts_extend = { "sources.default" }
  }
}
```




まず、僕は日本語変換と同じ動作である、TABで選択肢を移動しENTERで決定するということに慣れていて、
それと同じにしたいという思いがあった。

なので、keymapのpresetをenterにしておき、
TABとS-TABに専用動作を割り当てた。
preset super-tabを参考にしているが、一個目のelse内で、
listが一個だったらTABで決定、それ以上だったら次の候補へ移動という動きになっている。(S-TABは逆)

また、completionのlistにて、初期選択をしないようにした。
これは、markdownとかテキストにて、日本語を書いている間に初期選択されたとき、
変換決定のつもりでENTER押したのに、候補を選択するENTERとみなされて全然関係ない文章を補完してしまうという危険性を避けるためだ。

日本語入力が問題なら、filetypeがmarkdownとかの場合にbufferからの補完を切ってしまえばいいのだが、
授業関連で、結構日本語と英語を混ぜて書くことが多いので、補完が全くないのは面倒。
そのため、初期選択をしないようにすることで対応した。
選択に一手多いように感じるがCtrl-yとかで選択するよりよっぽど楽。


その他は割とおまじない感あるが、
補完候補に枠線をつける設定と、cmdlineモードでの補完で、:wqとかをいちいち補完出さないようにした設定ぐらいかな、特殊なのは。

これは下記参考URLの1つ目で紹介されていた。
とても便利。

とりあえず、こんなところです。





参考
[Neovim補完プラグインblink.cmpの使い方とカスタマイズ | eiji.page](https://eiji.page/blog/neovim-blink-cmp-intro/)
[📜2025-04-02 nvim-cmpからblink.cmpに移行してみる - Minerva](https://minerva.mamansoft.net/Notes/%F0%9F%93%9C2025-04-02+nvim-cmp%E3%81%8B%E3%82%89blink.cmp%E3%81%AB%E7%A7%BB%E8%A1%8C%E3%81%97%E3%81%A6%E3%81%BF%E3%82%8B)
[Introduction | Blink Completion (blink.cmp)](https://cmp.saghen.dev/)


