---
layout: post
title: "NeoVimを見直して、Lua全面採用してみた。"
tags : [Mac, 環境構築, Vim]
date: 2025-08-07 14:44:44
---


この前下記のようにちょっとNeoVimでTelescopeを導入してみたが、
そこからNeoVimの設定自体をの見直してみようかと思った。
[ripgrepの導入と、Telescope.nvimを使ってみた](/2025/08/01/install-ripgrep-and-telescope-nvim)




今までの設定(init.vimやプラグイン管理)は、
まだNeoVimではなく、Vimのときから使っていた設定を少しずつ継ぎ足し継ぎ足ししてきたものだから、
いい加減今の世の中に合わせようというわけだ。


NeoVimを使い続けるなら、ここで2つの選択肢があって、
1. init.vimのままで、かつ、Vimでも使えるプラグインのみを採用する。
2. init.luaに乗り換え、かつ、NeoVimでしか使えないプラグインでも積極的に採用する。


今後NeoVimがなくなるってことは考えづらいので、将来性に関してはどちらも一緒かと思う。
ネックなのは、僕が別環境のPCを使ったときとかに、自分の設定があり過ぎると移行が大変かもという心配。
ならばVimの基本的な機能を習熟するほうが良いのではと思うのだけど、
かと言って今のこのMacでの環境で作業する時間も長いのでどうしようかなって思っていたのだが、

Analysis Paralysisに陥るより、Act now, think laterのが良いこともある

という言葉を耳にして、
思い切ってLuaに飛び込んでみようかと思った。


なお、設定はいつものように dotfilesとしてgithubにあげてある。

[ryotakato/dotfiles](https://github.com/ryotakato/dotfiles)




### 環境情報

まずは環境情報。
MacBookAir の、2025 M4 

```bash
$ sw_vers
ProductName:		macOS
ProductVersion:		15.6
BuildVersion:		24G84
```


### init.luaへの移行

init.luaの書き方をゼロから学んだほうが良いのかもしれないが、
一旦移行を先にするため、最近らしく、ChatGPTにinit.vimを食わせて、
同等のinit.luaを出力してもらった。

その際、使っていない設定なども消去したので、こんな感じ。
まだ細かい部分は見直せるかもしれないが、一旦これでよい。
colorschemeはWombatを愛用しているが、プラグインとしていれるみたいなので、後回し。


```lua
-- ===============================
-- Basic Settings
-- ===============================
local opt = vim.opt
local g   = vim.g
local map = vim.keymap.set

-- Encoding
opt.fileencodings = { "ucs-bom", "utf-8", "default", "sjis", "latin1", "iso-2022-jp", "euc-jp" }

-- ===============================
-- Search
-- ===============================
opt.ignorecase = true    -- Ignore case by default
opt.smartcase = true     -- Override ignorecase if uppercase present
opt.wrapscan = true      -- Search wraps around the file
opt.hlsearch = true      -- Highlight search results

-- ===============================
-- View
-- ===============================
opt.number = true        -- Show line numbers
opt.title = true         -- Show file name in title
opt.ruler = true         -- Show cursor position
opt.list = true          -- Show invisible chars like EOL
opt.textwidth = 0        -- No automatic line wrap

-- ===============================
-- Tabs & Indent
-- ===============================
opt.tabstop = 4          -- Tab width
opt.shiftwidth = 4       -- Indent width
opt.expandtab = true     -- Convert tabs to spaces
opt.autoindent = true    -- Copy indent from current line

-- ===============================
-- Status line
-- ===============================
opt.showcmd = true       -- Show command in bottom-right
opt.laststatus = 2       -- Always show status line

-- ===============================
-- Clipboard
-- ===============================
opt.clipboard:append({ "unnamed", "unnamedplus" })

-- ===============================
-- Files
-- ===============================
opt.backup = false       -- Disable backup files

-- ===============================
-- UI / Editing
-- ===============================
opt.showmatch = true     -- Highlight matching brackets
opt.foldmethod = "marker"-- Folding method
opt.backspace = { "indent", "eol", "start" }
opt.wrap = false         -- Disable line wrap

-- ===============================
-- Keymaps
-- ===============================

-- Escape from terminal mode
map("t", "<ESC>", "<C-\\><C-n>", { silent = true })

-- Double ESC to clear search highlight
map("n", "<ESC><ESC>", ":nohlsearch<CR><ESC>", { silent = true })

-- Disable macro recording with q and remap to zq
map("n", "q", "<Nop>")
map("n", "zq", "q")
map("n", "@", "<Nop>")
map("n", "z@", "@")

-- Disable arrow keys in insert and normal mode, use gt/gT for tab switch
map({ "i", "n" }, "<Right>", "<Nop>")
map({ "i", "n" }, "<Left>", "<Nop>")
map({ "i", "n" }, "<Up>", "<Nop>")
map({ "i", "n" }, "<Down>", "<Nop>")
map("n", "<Right>", "gt")
map("n", "<Left>", "gT")

-- Swap ; and : in Normal & Visual mode
map({ "n", "v" }, ";", ":")
map({ "n", "v" }, ":", ";")

-- ===============================
-- Python host program
-- ===============================
g.python3_host_prog = vim.fn.expand("~/nvim_python3/.venv/bin/python3")
```




### filetype毎の設定


今まで、after/indent配下にC言語やMakefile用の設定を置いていたが、
NeoVimだとftplugin配下が推奨みたいなので、
そちらにc.lua, make.luaを作って下記ファイルを置く

c.lua

```lua
vim.opt_local.expandtab = true
vim.opt_local.tabstop = 2
vim.opt_local.shiftwidth = 0
vim.opt_local.softtabstop = 0
vim.opt_local.autoindent = true
```

make.lua
```lua
vim.opt_local.expandtab = false
vim.opt_local.tabstop = 8
vim.opt_local.shiftwidth = 0
vim.opt_local.softtabstop = 0
vim.opt_local.autoindent = true
```


### プラグイン管理

プラグインはdein.vimをやめて、
lazy.nvimにした

[folke/lazy.nvim](https://github.com/folke/lazy.nvim)

Documentに従って、階層構造は下記にした(plugins配下は後で入れたやつ。後述)

```bash
nvim/
├── init.lua
├── ftplugin
│   ├── c.lua
│   └── make.lua
└── lua
    ├── config
    │   └── lazy.lua
    └── plugins
        ├── im-select.lua
        ├── lightline.lua
        └── telescope.lua

```



まずlazy.nvimを導入するには、
init.luaの最後に下記を記載し、

```lua
-- ===============================
-- Plugin management
-- ===============================
require("config.lazy")
```

次にlua/confog/lazy.luaで下記を記載
(ほぼ公式の通り)
mapleaderは今回からspaceにした。
USキーボードだと無駄にspace広いから有効活用したいし。


```lua
-- Bootstrap lazy.nvim
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not (vim.uv or vim.loop).fs_stat(lazypath) then
  local lazyrepo = "https://github.com/folke/lazy.nvim.git"
  local out = vim.fn.system({ "git", "clone", "--filter=blob:none", "--branch=stable", lazyrepo, lazypath })
  if vim.v.shell_error ~= 0 then
    vim.api.nvim_echo({
      { "Failed to clone lazy.nvim:\n", "ErrorMsg" },
      { out, "WarningMsg" },
      { "\nPress any key to exit..." },
    }, true, {})
    vim.fn.getchar()
    os.exit(1)
  end
end
vim.opt.rtp:prepend(lazypath)

-- Make sure to setup `mapleader` and `maplocalleader` before
-- loading lazy.nvim so that mappings are correct.
-- This is also a good place to setup other settings (vim.opt)
vim.g.mapleader = " "
vim.g.maplocalleader = "\\"

-- Setup lazy.nvim
require("lazy").setup("plugins")
```



ここでNeoVimを再起動するとlazy.nvimがインストールされる。
この時点だと、まだplugins配下に何も入っていないので、何もないよっていうエラーが出るが、気にしない。





Nerd Fontをオプションで入れたほうがよいとあったので、
Nerd Fontを取り込んだ、HackGen(白源)フォントを採用
仕事の際はRicty Discordを好んでいたので、その一部がinspireされているのが嬉しい。

[yuru7/HackGen: Hack と源柔ゴシックを合成したプログラミングフォント 白源 (はくげん／HackGen)](https://github.com/yuru7/HackGen)

```bash
$ brew install font-hackgen-nerd
```

Warpの設定でfontを変更する。(HackGen Console NFってやつ。fontサイズは14にした)




### 各種プラグインの移行

プラグインを見直して、
どうしても残したかったのを考えると、

* im-select.nvim
* lightline.vim
* telescope.nvim

かな。telescopeはこの前入れたばっかりだから実質2つ。断捨離大事。
im-selectはNormal modeにしたときに日本語のままになっているのがすごい不快なので、これは必須。
lightlineは、今はlualineってのがあるらしいが、どうもairline系らしく、大きく変わるのは嫌なのでlightlineを残す。



ということで、上記で記したpluginsディレクトリ配下に、
下記3つのファイルを配置
ファイル名は何でもいいのだけど、分かりやすいように。


im-select.lua
```lua
return {
  "keaising/im-select.nvim",
  config = function()
    require("im_select").setup({
      default_im_select  = "com.apple.keylayout.ABC",
    })
  end,
}
```



lightline.lua
```lua
return {
  'itchyny/lightline.vim',
  config = function()
    vim.g.lightline = {
      colorscheme = 'wombat',
    }
  end,
}
```

telescope.lua
```lua
return {
  'nvim-telescope/telescope.nvim', tag = '0.1.8',
  cmd = { "Telescope" },
  dependencies = { 'nvim-lua/plenary.nvim' },
  keys = {
    { '<leader>ff', '<cmd>Telescope find_files<cr>', desc = 'Telescope Find files' },
    { '<leader>fg', '<cmd>Telescope live_grep<cr>', desc = 'Telescope Live grep' },
    { '<leader>fb', '<cmd>Telescope buffers<cr>', desc = 'Telescope Buffers' },
    { '<leader>fh', '<cmd>Telescope help_tags<cr>', desc = 'Telescope Help Tags' }
  },
  config = function() 
    local actions = require("telescope.actions")
    require("telescope").setup {
      defaults = {
        initial_mode = "normal",
        mappings = {
          i = {},
          n = { ["q"] = actions.close }
        }
      }
    }
  end
}
```

telescopeの設定は下記が分かりやすかった。
[Configuration Recipes · nvim-telescope/telescope.nvim Wiki](https://github.com/nvim-telescope/telescope.nvim/wiki/Configuration-Recipes)




lazy.nvimのすごいところはNeoVim再起動しなくても読み込んでくれるところだけど、
なんとなくいつも再起動してしまう。
まだNeoVimから抜けずに作業するってことができない。




### 新規プラグイン採用

#### Colorscheme

Colorschemeとしては、Wombatを愛用しているので、それのNeoVimプラグインであるwombat.nvimはいれるとして、
kanagawa.nvimのkanagawa-waveというのが結構良かったので、両方入れた。
init.lua で設定を書けばそのcolorschemeが反映されるので、
一旦kanagawa-waveを試してみて、飽きたらwombatにする。

[ViViDboarder/wombat.nvim: A wombat colorscheme for Neovim with lush](https://github.com/ViViDboarder/wombat.nvim)
[rebelot/kanagawa.nvim: NeoVim dark colorscheme inspired by the colors of the famous painting by Katsushika Hokusai.](https://github.com/rebelot/kanagawa.nvim)



#### ファイラー


ファイラーはyaziを使ってみたかったので、まずいれる。(そのほか必要なものも)
[sxyazi/yazi: 💥 Blazing fast terminal file manager written in Rust, based on async I/O.](https://github.com/sxyazi/yazi)


```bash
$ brew install yazi ffmpeg sevenzip jq poppler fd ripgrep fzf zoxide resvg imagemagick


$ yazi --version
Yazi 25.5.31 (Homebrew 2025-05-30)
```


次にそれをNeoVimで使うためのプラグインを入れる。

[mikavilpas/yazi.nvim: A Neovim Plugin for the yazi terminal file manager](https://github.com/mikavilpas/yazi.nvim)

yazi.lua
```lua
return {
  'mikavilpas/yazi.nvim', 
  event = "VeryLazy",
  dependencies = { 'nvim-lua/plenary.nvim', lazy = true },
  keys = {
    { '<leader>yz', '<cmd>Yazi<cr>', desc = 'Open yazi at the current file' },
    { '<leader>yw', '<cmd>Yazi cwd<cr>', desc = "Open the file manager in nvim's working directory" },
    { '<leader>yt', '<cmd>Yazi toggle<cr>', desc = 'Resume the last yazi session' },
  },
  opts = {
    open_for_directories = false,
    keymaps = {
      show_help = "<f1>",
    },
  },
}
```

ファイラーなんだから、<leader>yzとかy始まりではなくf始まりのほうがいいかと思うが、それだとtelescopeとぶつかるしな。
telescopeをtに変えるのがいいのかもだけど、t押しづらい気がするんだよね。
まあ、そんなこといったらyzもだけど。
とりあえず使ってから考える

ちなみに、NeoVimからyaziを開くと画像がプレビューされない問題がある。
yazi.nvimを使うんじゃなくて、terminalを開いてyaziを実行しても同じ。
NeoVim側の問題らしく、色々すれば回避策もあるみたいだが、一旦そこまでやらない。




#### Git連携

最近LazyGitを使うようになったので、プラグインいれる。
[kdheepak/lazygit.nvim: Plugin for calling lazygit from within neovim.](https://github.com/kdheepak/lazygit.nvim)


lazygit.lua
```lua
return {
  'kdheepak/lazygit.nvim', 
  lazy = true,
  cmd = {
    "LazyGit",
    "LazyGitConfig",
    "LazyGitCurrentFile",
    "LazyGitFilter",
    "LazyGitFilterCurrentFile",
  },
  dependencies = { 'nvim-lua/plenary.nvim' },
  keys = {
    { '<leader>lg', '<cmd>LazyGit<cr>', desc = 'LazyGit in the current working directory.' },
    { '<leader>lc', '<cmd>LazyGitCurrentFile<cr>', desc = 'LazyGit in the project root of the current file.' },
  },
}
```

lgは打ちやすいからこのままでいいかな。lcは使うことないかもだけど、とりあえず設定しておく。

ちなみに、LazyGitを開いてESCを使おうとすると、元のNeoVimのキーマッピングが奪ってしまっているのでESCキーが効かないという問題に出会ったし、
対処している人もいたが、よく考えたら僕はLazyGitの画面でESC使うことなかったので、一旦このままでいく。


どうでもいいけど、lazy.nvimとか、lazygit.nvimとか、名前紛らわしいよ。

[lazygit.nvimでESCキーがNeovimのノーマルモード変更にとられてしまい利用できない - Minerva](https://minerva.mamansoft.net/Notes/%F0%9F%93%9Dlazygit.nvim%E3%81%A7ESC%E3%82%AD%E3%83%BC%E3%81%8CNeovim%E3%81%AE%E3%83%8E%E3%83%BC%E3%83%9E%E3%83%AB%E3%83%A2%E3%83%BC%E3%83%89%E5%A4%89%E6%9B%B4%E3%81%AB%E3%81%A8%E3%82%89%E3%82%8C%E3%81%A6%E3%81%97%E3%81%BE%E3%81%84%E5%88%A9%E7%94%A8%E3%81%A7%E3%81%8D%E3%81%AA%E3%81%84)



#### プラグインまとめ

結局いれたのは、依存関係も含めるとこんな感じ
今後増えたら書き足していくかも。

* プラグイン管理
lazy.nvim
plenary.nvim

* 便利系
im-select.nvim
telescope.nvim 
lightline.vim
lazygit.nvim 
yazi.nvim

* Colorsheme
kanagawa.nvim 
wombat.nvim
lush.nvim





### Bram Moolennar追悼



なお、この過程で下記記事を見かけた。
Vim開発したBram Moolenaarが2023年に亡くなったのは聞いていたが、
下記は非常に良い内容だと思う。R.I.P.
[追悼 Bram Moolenaar ～Vimへの情熱と貢献を振り返る &#124; gihyo.jp](https://gihyo.jp/article/2023/11/memorial-to-bram-moolenaar)




