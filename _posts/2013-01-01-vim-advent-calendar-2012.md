---
layout: post
title: "なんでもhjkl for Windows"
tags : [Vim]
date: 2013-01-01 02:15:10
---

この記事は<a href="http://atnd.org/events/33746">Vim Advent Calendar 2012</a>の32日目の記事です。
前日は[@kaneshinth](https://twitter.com/kaneshinth)さんの<a href="http://kaneshin.hateblo.jp/entry/vim-advent-calendar-2012">Vimプラグインの拡張機能プラグインを作ってVimをさらに使いやすくしよう - 29th Sta.</a>です。
<p>あけましておめでとうございます！<br>皆さん、2012年はしっかりVimを楽しみましたか？<br>今年も良いVim年になるよう毎日Vimを使っていきたいですね。
<br>この記事書いてる横で2012年の紅白やってましたが、<br>館ひろしの「嵐を呼ぶ男」と、美輪明宏の「ヨイトマケの唄」が最高にカッコ良かったです。</p>
<p>さて、2013年あけて第一回目の記事は、<br>WindowsをVim一色に染め上げることです。</p>

仕事ではWindowsを使っており、
当然[@kaoriya](https://twitter.com/kaoriya)さんのGVimには大変お世話になっているのですが、
他のツールが、いかんせん使いづらいです。
僕もVimmerのはしくれとして、日々のカイゼンは重要だと思っているので、
使いやすいようにしていきたいと思います。
題して、「なんでもhjkl for Windows」

<p>※なお、本記事ではインストールなどの導入は書きません。書いてるとそれだけで一個の記事になりそうだからです。<br>この記事の目的はあくまで&quot;紹介&quot;です。<br>ただし、要望があれば or 気が向いたら、個別に書く予定です。</p>
<hr>
<h2>対象</h2>
<ul style="margin-left:20px;"><li>Windows XP</li><li>Widnows 7</li></ul>
<h2>目次</h2>
<ul style="margin-left:20px;"><li><a href="#browser">ブラウザ</a><ul style="margin-left:20px;"><li><a href="#chrome">Google Chrome</a></li><li><a href="#ff">Fire Fox</a></li><li><a href="#ie">Internet Explorer</a></li></ul></li>
<li><a href="#ide">IDE</a><ul style="margin-left:20px;"><li><a href="#eclipse">Eclipse</a></li><li><a href="#vs">Visual Studio</a></li><li><a href="#idea">IntelliJ IDEA</a></li></ul></li>
<li><a href="#other">その他</a><ul style="margin-left:20px;"><li><a href="#excel">Excel</a></li><li><a href="#tb">Thunderbird</a></li></ul></li>
<li><a href="#extras">番外：魔境編</a><ul style="margin-left:20px;"><li><a href="#alttab">Alt + Tab</a></li><li><a href="#explorer">Explorer</a></li><li><a href="#sqlserver">SQL Server</a></li><li><a href="#ipmsg">IP Messenger</a></li></ul></li>
<li><a href="#summary">まとめ</a></li></ul>
<hr>
<h2 id="browser">ブラウザ</h2>
<p>まずはブラウザから。</p>
<h3 id="chrome">Google Chrome</h3>
<p>僕はGoogle Chromeを愛用していますが、当然VimLikeなキーバインドが欲しくなりました。<br>「絶対、誰かがVimLikeな拡張機能を作っている筈だ！」という<s>他人まかせな</s>車輪の再発明を防ぐ思いをこめて検索すると、<br>ちゃんとあるではないですか！</p>
<p><a href="https://chrome.google.com/webstore/detail/vichrome/gghkfhpblkcmlkmpcpgaajbbiikbhpdi">Google ウェブストア - Vichrome</a>
</p>
<p>これに出会うまでは、Vimiumという拡張機能を利用していたのですが、<br>日本語の扱いができなかったので、
Vichromeを見つけてから、すぐに乗り換えました。
</p>
<p>ここまで書いて思いましたけど、<br>VimmerでChrome使いなら、大抵の人が知ってるのではないでしょうか？
</p>
<p>作者様ページ<br><a href="http://k2nr.blogspot.jp/2011/09/vichrome-extension.html">ニートなプログラマが世界を変える: Vichrome : vim風インタフェースを実現するChrome Extension</a>



</p>
<h3 id="ff">Fire Fox</h3>
<p>Chromeが来たら、当然FFもですね。<br>これを書かなかったら、FF使いからクレームが来そうです。<br>僕はメインでは使っていないですが、色々便利なアドオンがあるので、Webアプリを開発する上では重宝します。</p>
<p>で、アドオンなのですが、<br>Vimparatorがくると思った？思った？残念！<br>Pentadactylだよ！</p>
<p><a href="https://addons.mozilla.org/ja/firefox/addon/pentadactyl/">Pentadactyl :: Add-ons for Firefox</a>
</p>
<p>Vimparatorの開発者たちが、政治的な理由から仲違いして、
VimparatorをフォークしたのがPentadactylです。<br>「政治的な理由」を調べてみると、<br><a href="http://hitskey.biz/archives/1634">フリーソフトウェアコミュニティーの運営における話 | 明生流 - Myosho Ryu -</a><br>のように考察しているサイトもあるので、結構面白いです。<br>オープンソースのプロジェクト運営というのは大変ですなー。
</p>
<p>ちなみに、&quot;Pentadactyl&quot;は「五指状の」という形容詞です。
マウスを使わないVimmerにぴったりの名前ですね。
</p>
<p>作者様ページ<br><a href="http://5digits.org/">Home — Dactyl Home</a>


</p>
<p>あと、先ほど少し書いたVimiumですが、<br>FFにもアドオンとしてありますので、<br>ChromeでVimium使っている人はFFでも使ってみるといいですね。</p>




<h3 id="ie">Internet Explorer</h3>
<p><s>IEなんてなかった</s></p>
<p>いやいや、当然、Windowsなのですから、
IEもVim Likeにしたいと思うじゃないですか？</p>
<p>ところが、やってる仕事がIEで見るサイトの開発なもんで、<br>IEに何か入れたりいじったりすることが禁止されているわけなんですよ。</p>
<p>ですので、ぶっちゃけ探してません。<br>IEユーザーの人、ほんとすみません。(どれほどいるのか・・・)<br>後述するAutoHotkeyを使えばできそうな気はしてますが、今やってる案件を離れたらですね。<br>(というか、IE限定サイトの開発って、今更感たっぷりですが、仕事なので文句は言えません。)</p>




<hr>
<h2 id="ide">IDE</h2>
<p>IDEはすでにVimLikeにしている人も多いので、さらっと行きます。</p>
<h3 id="eclipse">Eclipse</h3>
<p>選択肢として、</p>
<ul style="margin-left:20px;"><li>Vrapper</li><li>外部エディターでGVim</li><li>Eclim</li></ul>
<p>などあるのですが、僕はVrapperを使ってます。<br>Vrapperはyuroyoroさんとかbasyuraさんとかが以前取り上げてますね。説明不要かと思います。(なんて人任せな。。。)<br><a href="http://yuroyoro.hatenablog.com/entry/20100218/1266477264">Eclipseのキーバインドをvim風にできるVrapperが素晴らしすぎる件について - ( ꒪⌓꒪) ゆるよろ日記</a><br><a href="http://d.hatena.ne.jp/basyura/20100913/p1">Eclipseのキーバインドをvim風にできるVrapperが &quot;マジで&quot; 素晴らしすぎる件について - basyura’s blog</a></p>
<p>作者様ページ<br><a href="http://vrapper.sourceforge.net/home/">Vrapper &mdash; Vim-like editing in Eclipse</a></p>


<h3 id="vs">Visual Studio</h3>
<p>仕事では、C#もJavaもJavaScriptもSQLもなんでもござれと書いてるのですが、<br>C#はVisual Studio 2010を使って書いてます。(これもプロジェクトによって決まっているため)</p>
<p>少しでもVimLikeにしようと探したら、当然のようにありました。</p>
<p><a href="http://visualstudiogallery.msdn.microsoft.com/59ca71b3-a4a3-46ca-8fe1-0e90e3f79329">VsVim 拡張機能</a>
</p>
<p>これでC#やF#でも怖くないですね！
VimコミュニティにどれくらいMicrosoft畑の人がいるのかは分かりませんが。</p>
<p>作者様Github<br><a href="https://github.com/jaredpar/VsVim">jaredpar/VsVim</a></p>



<h3 id="idea">IntelliJ IDEA</h3>
<p>プロジェクトに直接関係ないプログラムはEclipseよりIntelliJ IDEAで書いてます。(JavaとかGroovyとか)<br>Eclipseより使いやすくていいですよね！<br>ですけど、使用頻度が低くてVimLikeなプラグインは使ってません。</p>
<p>2013年中にはなんとかしたいと思い、軽く探してみたら、<br>書いてる方が見つかりました。<br><a href="http://ruzia.hateblo.jp/entry/20081025/1224900125">IdeaVIMが熱い - 守破離</a><br>「IntelliJ IDEA上でVIMエンジン実装しちゃってる感じのもの凄い PlugIn」という記述が気になります。<br>導入したら、また別個の記事を各予定ですので、少々お待ちを。</p>
<p>作者様ページ<br><a href="http://ideavim.sourceforge.net/">VIM Emulator Plugin for IntelliJ IDEA</a></p>



<hr>
<h2 id="other">その他</h2>
<p>その他大事なもの</p>
<h3 id="excel">Excel</h3>
<p>「日本人ならみんな大好きExcel」(棒)<br>そんなExcelにもVimLikeなキーバインドアドオンを作った人がいました。</p>
<p><a href="http://www.moongift.jp/2012/04/20120404-3/">もしvi/Vim使いがExcelを使ったら「Vimxls」|オープンソース・ソフトウェア、ITニュースを毎日紹介するエンジニア、デザイナー向けブログ</a>
</p>
<p>って、みんな知ってる？知ってたらごめん。<br>だけど、僕はこれでExcelの操作が三倍になりました。そのあとなんか赤くなったように見えるのは気のせいです。
</p>
<p>もちろんExcelなので、Vimと違うところが多々ありますが、それを差し引いても使いやすいです。<br>特にvでシート移動できるとか、最高なんですが。  
</p>
<p>現在ver0.8まで開発しているようですが、<br>Excel 2003 では動かないので、Windows XP ユーザーはver0.6を探してきて落とすほうがよいでしょう。  
</p>
<p>作者様Twitter<br><a href="https://twitter.com/Vimxls">Vimxls (Vimxls) on Twitter</a>
</p>



<h3 id="tb">Thunderbird</h3>
<p>会社でのメーラーはThunderbirdです。<br>操作自体は特にデフォルトで問題ないのですが、やはりメールの文面はデフォルトだと書きづらいです。<br>そこで、Thunderbirdのアドオンを使って、GVimで書けるようにしましょう。  </p>
<p><a href="http://kaworu.jpn.org/kaworu/2011-01-30-1.php">Thunderbirdでもvimでメールを３倍速く編集したい External Editor</a></p>
<p>メールを書くときに、Ctrl + e を押せば、<br>設定したエディターで書けるというアドオンです。<br>GVim実行ファイルのパスを設定すれば、GVimで書けるので、全く違和感なくメールを書けるようになりました。</p>
<p>作者様サイト<br><a href="http://globs.org/articles.php?pg=2&amp;lng=en">Globs site -  External Editor - Usage</a></p>



<hr>
<h2 id="extras">番外：魔境編</h2>
<p>ここからが本編です。<br>※Shougoさんに敬意を表して、発表デザインパターンの一つ、「おまけが本編です」パターンを使用しました。</p>
<h3 id="alttab">Alt + Tab</h3>
<p>タスク切り替えのショートカットであるAlt + Tab<br>Windows標準のは使い勝手が良くないので、VimLikeな切り替えを目指して探してみたところ、このソフトが良さそうです。  </p>
<p><a href="http://www.gigafree.net/utility/task/cltc.html">cltc - ｋ本的に無料ソフト・フリーソフト</a></p>
<p>こんな感じ</p>
<p><img src="{{ BASE_PATH }}/images/2013/01/01/vim-advent-calendar-2012-1.png" alt="cltcってこんなの" title="cltcってこんなの" /></p>
<p>うひょー、Minecraftたのしー！</p>
<p>じゃなくて、<br>cltcはただの切り替えソフトなので、VimLike用に作られた訳じゃないのですが、<br>キーバインドをカスタマイズできます。<br>インストールするとcltc.iniというファイルができるのですが、<br>このファイル内にカスタマイズのキーバインドを設定します。
</p>
<p><a href="https://github.com/ryotakato/dotfiles/blob/master/cltc.ini">dotfiles/cltc.ini</a></p>
<p>設定例は上記Githubに置いておきますが、<br>軽く説明すると、<br>[Key]パラグラフ？でキーバインドを設定します。</p>
<pre style="background-color:black;color:white;padding:3px 10px;"><code class="lang-config">UseNewKey3=1
Mod3=
Vk3=j
Cmd3=curdnl
UseNewKey4=1
Mod4=
Vk4=k
Cmd4=curupl</code></pre>
<p>Vk3が上移動、Vk4が下移動で、
それぞれのModがモディファイアキーです。
ここではモディファイアキーを無しにして、jとkに割り当ててます。<br>今のところ、これ以外はほとんど設定していないのですが、十分だと思います。</p>
<p>ただし、一個だけ副作用が！<br>cltcは、入力したキーでタスクを絞り込む機能を持っています。(というか、それが主機能みたい)<br>例えば、eと入力したら、&quot;e&quot;を含むタスクのみを表示します。<br>ちょうどUniteのような感じですね。<br>ですので、jk を割り当ててしまうと、jとkを入力できないという罠が。<br>そもそもあまりタスクを立ち上げない僕としては特にしぼらなくてもjkの選択だけで十分なのですが、<br>気になる人はモディファイアキーを割り当てるなど対応してください。(えー)  </p>
<p>作者様ブログ<br><a href="http://d.hatena.ne.jp/cogma/">色々書くとこ(仮)</a>  

</p>
<p>ちなみに、ずっとcltcは何て読むのか分かりませんでしたが、
今回作者様のブログを見てたら、分かりました。(以下のコメント欄)<br><a href="http://d.hatena.ne.jp/cogma/20060215">2006-02-15 - 色々書くとこ(仮)</a><br>「コルトシー」と呼ぶのが良さそうです。<br>「こるとぅる」はダメみたいww  </p>
<p>参考サイト<br><a href="http://d.hatena.ne.jp/gan2/20070702/1183395916">cltc というタスク切り替えソフトが便利な件 - gan2 の Ruby 勉強日記</a>
</p>



<h3 id="explorer">Explorer</h3>
<p>ファイラーは、vimfiler一択だろ！<br>って声が聞こえてきそうですが、僕はvimfilerとExplorerを併用してます。<br>Vimで編集するファイルとかはvimfilerで、それ以外はExplorerで。<br>これ自体は結構気に入ってる使い方なのですが、<br>操作体系は別々だとストレス溜まります。</p>
<p>なので、AutoHotkey + 独自スクリプトでExplorerにvimfiler的なキーバインドを使えるようにしてみました。</p>
<p><a href="http://ahk.xrea.jp/">AutoHotkeyを流行らせるページ</a><br><a href="https://github.com/ryotakato/dotfiles/blob/master/vim.ahk">dotfiles/vim.ahk</a>  
</p>
<p>AutoHotkeyに関しては説明しているとそれだけでいくつも記事が書けそうですが、<br>一行でいうと、Windowsのカスタマイズツール？スクリプト環境？です。(説明できてねぇorz)  
</p>
<p>二個目のリンクは、僕が書いたスクリプトです。<br>AutoHotkeyをインストールし、vim.ahkを起動するだけで、あなたのExplorerがVimLikeに！<br>しかも、今ならなんとWindowsキーから立ち上がるスタートメニューもhjklで操作できるスクリプト付き！<br><s>「でも、お高いんでしょう？」</s> 
</p>
<p>Windowsの起動時に立ち上がるようにしておけば、快適です。
</p>
<p><a href="http://qttabbar-ja.wikidot.com/tutorial-howto">QT TabBar の使い方 - QuizoApps</a>
</p>
<p>僕はさらにExplorerを拡張するQTTabBarもインストールしているので、Explorerがタブにまとまり、こんな感じ  </p>
<p><img src="{{ BASE_PATH }}/images/2013/01/01/vim-advent-calendar-2012-2.png" alt="オレのExplorerが暴走するぅぅ!!" title="オレのExplorerが暴走するぅぅ!!" /></p>
<p>って、この画像じゃhjkl分かんないじゃん！</p>
<p>ちなみに、新規ディレクトリ作成(vimfilerでいうところの&quot;K&quot;)なのですが、<br>Windows7で試したら動きませんでした。<br>帰省から戻ったら、すぐに直しますが、<br>ショートカットを割り当ててるだけなので、多分みれば誰でも直せます。<br>まだまだ色々そろってないスクリプトなので、ご容赦を。</p>

<p>AutoHotkey本家サイト<br><a href="http://www.autohotkey.com/">AutoHotkey</a></p>
<p>QTTabBar本家サイト<br><a href="http://qttabbar.sourceforge.net/">QTTabBar</a></p>




<h3 id="sqlserver">SQL Server</h3>
<p>SQL Server 2005を使っていて、<br>カラム名とか調べるのに、わざわざSQL Server Management Studio という重いソフトを立ち上げたくなくて、<br>Uniteプラグイン書いちゃいました。</p>
<p><a href="https://github.com/ryotakato/unite-sqlserver">ryotakato/unite-sqlserver</a></p>
<p>ただ単に、SQLServerのDBを取得して、テーブル一覧とカラム一覧を見るためのUniteプラグインです。<br>2005でしか試してないよ。<br>姉妹プラグインとして、MongoDBを操作するUniteプラグインも開発中ですので、興味があればそちらもどうぞー。</p>
<p><a href="https://github.com/ryotakato/unite-mongodb">ryotakato/unite-mongodb</a></p>
<p>以上、ステマじゃないただのマでした。
unite-mongodbは、ある程度作ったらちゃんと紹介記事書きます。</p>



<h3 id="ipmsg">IP Messenger</h3>
<p>いま、僕が一番欲しいものです。<br>IP Messengerを書くときにVimで書けたらなーと何度思ったことか！！
誰か情報ありませんか？<br>あったら、是非とも教えていただきたいものです。</p>
<p>なければ、今年の僕の目標はVimのIP Messengerプラグインを書くことになるかと思います。</p>



<hr>
<h2 id="summary">まとめ</h2>
<p>Windows系のVimLikeまとめ記事ということで、簡単に紹介させていただきました。<br>なんか書いてたら、個別には他の人が色々取り上げている気がするので、<br>既出ばっかりだろうけど、まあWindows環境を一から作るときのための一覧と思ってください。</p>
<p>様々なVimプラグインを見ると、Vimでなんでもできちゃうかもしれませんが、<br>そうは言っても他のツールを使うことは多いので、他のツールでVimLikeを目指しました。<br>ここで書いた以外にも色々やり方はあると思うので、<br>&quot;カイゼン&quot;したい人は試してみるとよいのではないでしょうか。<br>ちなみに、僕はWindowsよりMacより、Linuxが大好きです。</p>


明日の<a href="http://atnd.org/events/33746">Vim Advent Calendar 2012</a>の33日目の記事は、[@fukayatsu](https://twitter.com/fukayatsu)さんです。  



