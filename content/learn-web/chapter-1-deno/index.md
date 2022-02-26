+++
title = "Denoをインストール"
date = "2022-02-25T07:42:15+09:00"
draft = false
+++

# Denoをインストール

[deno.land](https://deno.land/#installation) のDenoをインストールする手順に従ってインストールしましょう。

インストールできたら、`~/.deno/bin` にパスを通しましょう。

```bash
# ~/.bashrc, ~/.zshrcに追記
export PATH="$HOME/.deno/bin:$PATH"
```

```bash
deno info
```

と打つと、依存のキャッシュなどがどこに保存されているかが表示されます。

```bash
$ deno info # Macの場合
DENO_DIR location: "/Users/USERNAME/Library/Caches/deno"
Remote modules cache: "/Users/USERNAME/Library/Caches/deno/deps"
Emitted modules cache: "/Users/USERNAME/Library/Caches/deno/gen"
Language server registries cache: "/Users/USERNAME/Library/Caches/deno/registries"
Origin storage: "/Users/USERNAME/Library/Caches/deno/location_data"
```

deno自体は

```bash
which deno
```

と打つと表示されます。

```bash
$ which deno # Macの場合
/Users/USERNAME/.deno/bin/deno
```

**denoをuninstallするとき**

上のように、キャッシュとdeno本体を消せば良いです。

```bash
# Macの場合
rm -rf /Users/USERNAME/Library/Caches/deno
rm -rf /Users/USERNAME/.deno
```

## VS Codeでの設定

Denoで検索するとDenoの拡張機能があります。これだけでformatter(コード整形)、linter(コードの規約に沿った指摘)、などが有効になります。

ただ、DenoとNode.jsは同じ `.ts` ファイルを使うので、ファイルにどちらの拡張を効かせるか決めてやる必要があります。ここではDenoは `.vscode` を用いてプロジェクト下でのみ有効化することにします。

[Denoの開発環境をVSCodeで作る【2021.4月版】](https://qiita.com/SuzuTomo2001/items/3d85eb92fcfff255ce46) を参考に進めてみましょう。基本的にはVS Codeの拡張でUserとWorkspaceタブがありますが、UserタブでEnable denoをチェックすると毎回有効化されてしまうのでチェックしない、ということがポイントです。

Lintなど、各種機能はチェックをつけて有効にしておきましょう。

#### feedback

フィードバックのリンクは [こちら](https://forms.gle/Yd7nN2FK5tTYP6Xp8)
