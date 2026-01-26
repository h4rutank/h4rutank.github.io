---
title: "Jekyllでブログはじめました"
date: 2026-01-20
categories: diary
---
### Overview
GitHub pagesとJekyllでブログを作成しました。
本記事ではその手順をご紹介します。
タダでブログを作成・公開できるので、気に入っています。

### Prerequisites
- Environment
    - Windows 11
- Software
    - [Git](https://git-scm.com/install/windows)
    - [Ruby](https://rubyinstaller.org/): WITH DEVKITをダウンロード
        - インストーラで"MSYS2 development toolchain"を選択
        - ⚠️パスが通っていることを確認  
            cmdで`ruby -v`と入力して、Rubyのバージョン情報が表示されればOK

### Quick Start
ブログの立て方を説明します。
1. GitHubでブログ用リポジトリを作成  
    [Quickstart for GitHub Pages](https://docs.github.com/ja/pages/quickstart)の手順1-9をもとに、ブログ用リポジトリを作成
    - 手順3のリポジトリ名: `<ユーザ名>.github.io`
    - 手順8-9のGitHub Pages設定について  
        Settings -> Code and automation -> Pages -> Build and deployment でSource: "Deploy from a branch"、Branch: "main/root"を選択<br>

1. Rubyのライブラリ管理ツールbundlerをインストール  
    * cmdで`gem install bundler`を実行<br>

1. Jekyllでブログ用プロジェクトの自動生成  
    ブログ用フォルダmyblogを置くディレクトリに移動して、以下のコマンドを実行
    ```batch
    jekyll new myblog
    cd myblog
    bundle install
    bundle add webrick
    ```
    jekyllコマンドが使えない場合はrubyのパスが通っていないかもしれません。  
    また、cmdで`where jekyll`を実行してjekyllのパスが表示されなければ、jekyllが正しくインストールされていません。
1. ローカルで動作確認  
    以下のコマンドを実行
    ```batch
    bundle exec jekyll serve
    ```
    ブラウザで[localhost:4000](http://127.0.0.1:4000)へアクセスすると、ブログが表示されます。
1. 作成したブログデータをGitHubへアップロード  
    以下のコマンドを実行
    ```batch
    git init
    git add .
    git commit -m "first commit"
    git branch -M main
    git remote add origin https://github.com/<ユーザ名>/<ユーザ名>.github.io.git
    git push -u origin main
    ```
    `git remote add origin`の引数のURLはGitHubリポジトリページのCode -> HTTPS に記載されています。
1. ブログ完成  
    ブラウザで**https://<ユーザ名>.github.io**へアクセスすると、ブログが表示されます。

### Post
ブログへ記事を投稿する手順を説明します。
1. 記事を作成  
    myblog直下の_mypostというフォルダに、記事を作成していきます。  
    記事はmarkdown形式のファイルで、投稿ごとにファイルを作成する必要があります。  
    たとえば、この記事のファイルの中身は以下の通りです。最初のヘッダ部分に記事のタイトル、投稿日、カテゴリなどの情報を記載します。
    ```markdown
    ---
    title: "Jekyllでブログはじめました"
    date: 2026-01-20
    categories: diary
    ---
    ### Overview
    GitHub pagesとJekyllでブログを作成しました。
    本記事ではその手順をご紹介します。
    タダでブログを作成・公開できるので、気に入っています。

    ### Prerequisites
    - Environment
        - Windows 11
    - Software
        - [Git](https://git-scm.com/install/windows)
        - [Ruby](https://rubyinstaller.org/): WITH DEVKITをダウンロード
            - インストーラで"MSYS2 development toolchain"を選択  
            - ⚠️パスが通っていることを確認  
            cmdで`ruby -v`と入力して、Rubyのバージョン情報が表示されればOK

    (以下略)
    ```

1. ブログの更新  
    記事のファイルを保存したら、ブログの更新内容をGitHubのリポジトリへコミット、プッシュします。
    ```batch    
    git add .
    git commit -m "コミットメッセージを自由に書いてください"
    git push
    ```

    すると、投稿がブログに反映されます。

### Conclusion
...というわけで、ブログを始めます。電力工学や組み込みに関する記事を書いていくつもりです。

Stay tuned!😊