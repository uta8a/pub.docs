+++
title = "技術選定について"
date = "2022-02-25T09:39:39+09:00"
draft = false
+++

# 技術選定について

## 本資料で使用する技術

- フロントエンド: [Deno](https://deno.land/)
- バックエンド: [Go](https://go.dev/)
- データベース: [PostgreSQL](https://www.postgresql.org/)
- ツール:
	- エディタ [VS Code](https://azure.microsoft.com/ja-jp/products/visual-studio-code/)
	- コンテナ [Docker](https://www.docker.com/)
	- コンテナ [docker-compose](https://github.com/docker/compose)
	- スクリプトランナー [jurassiscripts/velociraptor](https://github.com/jurassiscripts/velociraptor)
	- コミットメッセージの規約 [commitlint](https://github.com/conventional-changelog/commitlint)

また、この資料は [uta8a/maido](https://github.com/uta8a/maido) を使って作られています。

## なぜDenoを選択したか

今回、フロントエンドと開発用HTTPserverにDenoを使用しています。

Denoの他に選択肢として、

- Node.js
- そのままのJavaScriptをフロントエンドで使う(vanilla)

がありました。以下の観点からDenoを選んでいます。

- `fmt`, `lint`, `test` など、はじめてWeb開発する時に大切な要素が、denoをインストールするだけで `deno test` のようにすぐ使える
- TypeScriptがネイティブで動く

Denoを選択して少し不安な点は以下の通りです。(2022/02/25現在)

- VS Codeとの連携 (lspサーバの挙動が不安定でよく落ちる気がする、TypeScriptの型ヒントがマウスオーバーで出るが、単語の後ろに表示はまだ(？))
- Denoがこれからメジャーになるか分からないところで初心者向けハンズオンに投入していいのか

## なぜGoを選択したか

今回、バックエンドAPIサーバにGoを使用しています。

Goの他に選択肢として、Deno, Node.js, Python, PHP, Rust, Ruby on Rails, ... などがありました。以下の観点からGoを選んでいます。

- `test` がすぐ使える
- 割とポピュラー
- C言語とかPythonあたりと書き味が似ていると思うので、サークルの人たちに馴染みやすいのではないか
- 標準ライブラリが厚いので、`net/http` でサーバが書ける

Goを選択して少し不安な点は以下の通りです。(2022/02/25現在)

- 記事が少し古いGoに関するものが多く出回っているため、検索した時に間違った情報を掴む可能性がある。

## ツール類

- Docker: これはWeb開発に必須なので。
- スクリプトランナー: 開発のイテレーションを早める体験をしてほしいという気持ちから選択。
- commitlint: コミットメッセージに気をつける習慣づけを行うために導入。

#### feedback

フィードバックのリンクは [こちら](https://forms.gle/Yd7nN2FK5tTYP6Xp8)