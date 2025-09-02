---
layout: post
title: "TUIで操作できるTodoアプリケーションを検討"
tags : [Vim]
date: 2025-09-02 17:42:44
---


前々から、よいTODOアプリはないかなって思ってて、
色々試したりしたけど、結局今はiCouldのメモにTODO書いている。

とはいえ、決して使いやすいわけじゃないので、どうしたものかと思っていた。
MacでNeoVim環境を新しくしたり、Rustを入れたりしたので、改めて調べてみた。

色々見つかって、
特に、TODOのtxtを書くformatが存在することを知った。

[todotxt/todo.txt](https://github.com/todotxt/todo.txt)


ほう、シンプルなフォーマットながら、欲しいことはだいたいまとまっている。

ということで、これを扱えるツールはないものかと思ったら、
TUIで3種よさそうなのが見つかったので、試してみた。





### torudo (rust でインストール )

[maedana/torudo](https://github.com/maedana/torudo)

Rustで作られたtodo管理。

試してみた感じの感想は、

* カンバン形式の見た目はクール
* todoの新規追加ができないので、それはtxtファイルを自前で編集することになる(分割ウィンドウ使っている人向け感)
* Nvim連携が非推奨の方法みたい（NVIM_LISTEN_ADDRESS）
* 詳細ファイルをmd化しているのはすごい良い
* 完了したタスクを別ファイルにしているので、後で全部見られないのがきつい
* キーマッピングが変えることができない
* 気になった部分をIssueで聞いたけど、反応ないので、たぶんもう開発ストップしてる

ってな感じ。カンバン風はすごいいいんだけどね。


とりあえず採用は見送り。




### todotxt.nvim (nvimプラグイン )

NeoVimプラグインとしてどんぴしゃなものが見つかった。

[phrmendes/todotxt.nvim](https://github.com/phrmendes/todotxt.nvim)

試してみた感じの感想は、
* Nvimから操作しやすい
* キーマッピングで少しはまった そして、そのキーマッピングを該当バッファーのみに適用する方法がない
* テキストの見た目はシンプル
* 終わったタスクをクリーンするのが別コマンドなので、終わったタスクも眺められる。この仕組みは良いと思った。
* 良くも悪くもそれ以上のことができない

とりあえず試したときは、ソースをひっくり返しながら、
下記のようにlazy.nvimの設定ファイル書いて試したけど、
2つ目で挙げた、キーマッピングを設定したときに、そのキーマッピングを該当バッファのみに閉じ込めたいのに、その方法がないのは致命的。
だから、他のファイル開いているときでも誤爆して、テキストがおかしなことになる。これは相当ストレスなので、 採用見送り。


todotxt.lua

```lua

return {
  "phrmendes/todotxt.nvim",
  lazy = false,
  keys = {
    { '<leader>dn', '<cmd>TodoTxt new<cr>', desc = 'New todo entry' },
    { '<leader>dt', '<cmd>TodoTxt<cr>', desc = 'Toggle todo.txt' },
    { '<leader>dd', '<cmd>DoneTxt<cr>', desc = 'Toggle done.txt' },
  },
  config = function() 
    require("todotxt").setup({
      todotxt = vim.env.HOME .. "/todotxt.nvim/todo.txt",
      donetxt = vim.env.HOME .. "/todotxt.nvim/done.txt",
    })

    vim.api.nvim_create_user_command("TodoTxtSort", function(args)
        local cmd = args.fargs[1]

        if not cmd then
            require("todotxt").sort_tasks()
        elseif cmd == "priority" then
            require("todotxt").sort_tasks_by_priority()
        elseif cmd == "context" then
            require("todotxt").sort_tasks_by_context()
        elseif cmd == "project" then
            require("todotxt").sort_tasks_by_project()
        elseif cmd == "due_date" then
            require("todotxt").sort_tasks_by_due_date()
        else
            vim.notify("TodoTxtSort: Unknown command: " .. cmd .. ". Available: priority, context, project, due_date", vim.log.levels.ERROR)
        end
    end, {
        nargs = "?",
        desc = "TodoTxtSort commands (default: tasks, 'priority': priority, 'context': context, 'project': project, 'due_date': due date)",
        complete = function(arg)
            local cmds = { "priority", "context", "project", "due_date" }
            return vim.iter(cmds):filter(function(c) return c:find(arg, 1, true) end):totable()
        end,
    })

    vim.api.nvim_create_user_command("TodoTxtClean", function() require("todotxt").move_done_tasks() end, {
        nargs = 0,
        desc = "Move done tasks",
    })
    vim.api.nvim_create_user_command("TodoTxtState", function() require("todotxt").toggle_todo_state() end, {
        nargs = 0,
        desc = "Toggle todo state",
    })
    vim.api.nvim_create_user_command("TodoTxtCycle", function() require("todotxt").cycle_priority() end, {
        nargs = 0,
        desc = "Cycle priority",
    })

    vim.api.nvim_set_keymap("n", "<leader>dx", '<cmd>TodoTxtState<CR>', { noremap = true, buffer = bufnr, desc = "Toggle todo state", })
    vim.api.nvim_set_keymap("n", "<leader>dp", '<cmd>TodoTxtCycle<CR>', { noremap = true, buffer = bufnr, desc = "Cycle priority", })

  end
}
```


### todotui (go でインストール)

[yuucu/todotui](https://github.com/yuucu/todotui)


Goでつくられた扱いやすいTODOアプリ。

雑感は、

* プロジェクトやコンテクストを左側のパネルで一覧表示しているのはすごく見やすい
* 開始日を入れても一覧では見れず、編集しようとするとようやく見れる（なんなら入力したタイミングを開始日にしたり、開始する機能がほしい）
* 終わったタスクは別のところから見ないといけないので、間違えて完了にしてしまうと面倒
* キーマッピングが変えることができない

2,3個目がちょっとどうかと思うんだけど、
今回の3つの中では一番よいので、一旦採用。
NeoVimから容易に開けるように、toggletermの設定で下記を書いた
on_openは、普通の人なら別に不要だと思うけど、
僕はtoggleterm入ったときは基本normal modeにしているから、ここではあえてinsert mode(terminal mode)にしているってだけ。

```lua

    local Terminal  = require('toggleterm.terminal').Terminal
    local todotui = Terminal:new({ 
      cmd = "todotui ~/todotui/todo.txt", 
      hidden = true, 
      direction = "float", 
      on_open = function()
        vim.cmd('startinsert!')
      end
    })

    function _todotui_toggle()
      todotui:toggle()
    end

    vim.api.nvim_set_keymap("n", "<leader>do", "<cmd>lua _todotui_toggle()<CR>", { noremap = true, silent = true})


```


とりあえずこれで使っていく。




### 結局自分がTODOアプリに求めていることは何か？

今回調べてみて、色々考えたので、自分の中での要件を整理してみる。

* 容易な管理（いちいちブラウザログインしてとかやりたくない）
* カンバン形式？最初はいいと思ったけど、見やすければ別に必須じゃなさそう。
* 終わったタスクも眺めたい。
* タスクの依存関係を整理したい（そしてそれを画面でみたい？）
* txtファイルとして共有できる (けど、スマホで編集するのかと言われるとうーん）
* NeoVimから容易に操れる


こんなところかな。
Rustを最近触り始めて、RustでTUIアプリ作るにはratatuiってのがほぼ標準らしく、
そのうち自分で作ってもいいかも。




