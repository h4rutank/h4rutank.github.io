---
title: "どうやって仕事のメモを取るか"
date: 2026-02-11
categories: diary
---
### markdownでメモを取りたい
これまで、OneNoteでメモを取ってきたが、正直使いにくかった。<br>

* ワンページでしかメモを取れない
    * スクロールダウンしにくいので、長いメモには向いていない
    * ページの数は自然と多くなり、メモを探すのが困難になる
* クリックした位置に文字が入力される
    * フォーマットに統一感がなくなる
    * 画像挿入の位置・サイズ調整がやりにくい
    * 完璧主義のA型とは相性が悪い
    * カーソルの位置を見失う
* コピペで体裁が崩れる
    * 議事録をメールで送りたいのに、Outlookへコピペしたら箇条書きの点の位置が変になる
    * PDFでメモを保存すると、文書全体の存在感が薄くなる。（読みにくい）

...などなど、不満がたまっているので、新しいメモアプリを探すことにした。<br><br>

必須条件は3つ<br>
* markdown形式
* 画像挿入が容易
* 検索性が高い

学生時代、授業ノートをHackMDで取っていたこともあり、markdown形式が良い。<br>
マウスに手を伸ばして、タイピングを中断したくないので、画像のサイズ調整をキーボードで出来たら喜ばしい。<br>
あと、どのメモに何を書いてたか忘れてしまうので、カテゴリ分類や内容の一括検索がしたい。<br><br>


### VS codeでHackMDみたいなメモを取りたい
色々調べて、Visual Studio codeの拡張機能「**VS Notes**」を使うことにした。<br>
VS Notesを使うと、markdown形式でメモを取ることができる。<br>いや、メモを取るだけなら拡張機能を入れなくてもいいんだけど、VS Notesはカテゴリ整理とファイル名の自動記名を行ってくれる。<br>仕事のメモって、ファイル名に日付を入れたいけど、いちいち入力するのが面倒じゃないですか。こいつは自動でやってくれます。<br><br>

#### 導入手順
1. 必要な拡張機能をVS codeへ追加
    * [VS Notes](https://marketplace.visualstudio.com/items?itemName=patricklee.vsnotes): 今回の主役
    * [Auto-Open Markdown Preview](https://marketplace.visualstudio.com/items?itemName=hnw.vscode-auto-open-markdown-preview): 編集中のmdファイルが、どのように表示されるかをリアルタイムでプレビューしてくれる
    <br>

1. VS Notesの初期設定<br>[このページ](https://marketplace.visualstudio.com/items?itemName=patricklee.vsnotes#quick-start)のQuikc Startに沿って進めていく。
    * VS codeで`ctrl + shift + P`を押してコマンドパレットを開いて、`VSNotes: Run setup`を選択
    * 右下のポップアップで`Start`を押して、ファイルを保存する場所を設定
    * VS codeの再起動
    <br>
1. VS Notesのその他の設定
    * ファイル名のフォーマット設定: [参考1](https://marketplace.visualstudio.com/items?itemName=patricklee.vsnotes#filename-tokens)
    * 文書テンプレの設定: [参考2](https://marketplace.visualstudio.com/items?itemName=patricklee.vsnotes#custom-templates), [参考3](https://blog.chick-p.work/blog/vsnote-template)
        * VS codeの[File]->[Preferences]->[Configure Snippets]->[markdown]から`markdown.json`にテンプレの中身を追記
        * コマンドパレットを開いて、`Preferences: User settings open(json)`から`settings.json`へ`vsnotes.templates`の内容を追記（参考2をみてね！）
        <br>

#### 使い方
1. VS codeで`ctrl + shift + P`を押してコマンドパレットを開いて、`VSNotes: Create a New Note`を選択
    <br>
1. テンプレを選択して、ファイル名を入力
    <br>
1. ノートを書いていく
    * 基本的にmarkdown形式
    * 画像のサイズはhtmlを使う。`<img src="ファイル名" width="80%" height="auto">`とか。

    <img src="/img/IMG_0209" width="80%" height="auto">
    <br>
### 所感
満足している。<br>

ただし、**markdownのデメリット**も受け入れなければならない。
* 行間調整が手間。結局、`<br>`で改行しなきゃいけない。
* シンプルな機能しかない。色を付けたり、フォントの変更など、テキストの装飾ができない。
* ディスプレイが狭くなる。半分エディタ画面、半分プレビュー画面なので狭い。

あと、箇条書きの階層化がインデントだけで出来てしまうので、深い箇条書きをしてしまうのが良くない。客観的に分かりにくい文書になる。<br><br>

たちまち、仕事の効率化につながったので良しとする（広島弁）<br>効率化して、少しでも睡眠時間を増やしたい。