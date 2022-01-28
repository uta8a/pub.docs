# Rustを学ぼう

この文書は、 [The Rust Programming Language](https://doc.rust-lang.org/book/) を参考に、Rustはいいぞ！ということを伝えるために作った資料です。

## 目次

<!-- toc -->

## はじめに

初めてのプログラミング言語を学ぶときには、「その言語の良さが分からないと学ぶモチベーションが上がらない」でも、「言語の良さをきちんと知るにはその言語を少しは触る必要がある」というジレンマがあります。

そこで今回は、`if`や`for`などの基本的な部分+所有権というRustの特徴的な部分をざっと紹介した後で、Rustのよさである「安全性」「実行速度」「開発体験」をそれぞれ手を動かしながら体験してもらうという構成にしました。

つまり、先に簡単にRustの勉強をした後で、Rustの良さを知る、という流れです。

それでは始めていきましょう。

## ブラウザからRustを気軽に試す

Rustをインストールするのはとても簡単ですが、まずは気軽に試すためにブラウザだけで簡単なコードが試せる [Rust Playground](https://play.rust-lang.org/) を使ってみましょう。このPlaygroundはブラウザで入力したコードを裏でサーバに投げてコンパイルして実行して結果を返してくれるものです。

初めて使う方は以下のようなコードが表示されていると思います。この部分は自由に編集することができます。

```rs
fn main() {
    println!("Hello, world!");
}
```

これがRustのハローワールドです。main関数内に、標準出力を行うマクロ `println!` がありますね。マクロとは、他のコードを記述するためのコードを書く手段です。どのように他のコードを記述しているのかは [`println!` のドキュメント](https://doc.rust-lang.org/std/macro.println.html) から、`Macro std::println` の右側にある [`src`](https://doc.rust-lang.org/src/std/macros.rs.html#94-99) 部分を見てみると良いでしょう。

しかし現段階での理解は難しいと思うので、今は、関数は `!` なしで `func_name()` のように呼び出され、マクロは `!` ありで `macro_name!()` のように呼び出されるということを覚えていれば大丈夫です。

それではこのコードを実行してみましょう。左上の「`RUN`」ボタンを押してみてください。画面下半分のところに「`Progress`」で黒丸が表示され、少し時間が経つと「`Standard Error`」, 「`Standard Output`」部分に表示がされていると思います。「`Standard Error`」が標準エラー出力、「`Standard Output`」が標準出力です。

また、「`SHARE`」から「`Permalink to the playground`」をクリックすると、 [https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=7c4f223819334660c7915edb9830a15e](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=7c4f223819334660c7915edb9830a15e) のように書いたコードへのリンクが得られます。

使い方はこれで以上です。それでは、どの言語にも共通する`if`や`for`のような基本的な事柄をPlayground上で学んでみましょう。

<!-- TODO: もう少し例を追加する、実行について述べる、所有権の例を出してコンパイルエラーに慣れさせる -->

**確認テスト**

- [ ] Rust Playgroundの使い方(コードの書き方、実行の仕方、リンクの保存の仕方)を理解した
- [ ] 関数とマクロの違いをなんとなく理解した

## この後の進め方

この後はRustの基本的な部分と、所有権というRustの特徴的な部分を学びます。

基本的にはRust Playgroundのリンクを開き、実行し、説明文を読み、演習に答える、という流れです。演習では参考リンクを貼っているので、それを見てチャレンジすると良いと思います。

それではやっていきましょう！

## 変数

[こちらのPlaygroundへのリンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=473312ffaf20421980b8dd174b4a59ef) を開いて実行してください。

以下のようなエラーが出たと思います。

```
   Compiling playground v0.0.1 (/playground)
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 10;
  |         -
  |         |
  |         first assignment to `x`
  |         help: consider making this binding mutable: `mut x`
3 |     println!("x = {}", x);
4 |     x = 1000;
  |     ^^^^^^^^ cannot assign twice to immutable variable

For more information about this error, try `rustc --explain E0384`.
error: could not compile `playground` due to previous error
```

Rustは実行バイナリにコンパイルして実行する言語です。このエラーは、コンパイルできないよ！というエラーです。

エラー文を読んでみると、「cannot assign twice to immutable variable」(immutableな変数に2回代入はできない)とありますね。immutableは「不変」という意味です。Rustでは、変数はデフォルトで不変になっていて、再代入ができなくなっています。

代入したい場合は、エラーメッセージの「help: consider making this binding mutable: `mut x`」にあるように、`mut`キーワードを使います。

**演習**

- [問題へのリンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=35bb2c56c87d81d52d4a071d49add48a) を開いて、コンパイルが通るように「ここを書き換えよ」というコメントのある行を書き換えよ。
  - 参考: [TRPL: 変数と可変性](https://doc.rust-jp.rs/book-ja/ch03-01-variables-and-mutability.html)
  - [解答へのリンクはこちら](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=a6ff551ed8151ceb8a36784cc2dc39b9)

## データ型

静的型付き言語であるRustでは、値には型がついています。データ型の中にある「スカラー型」と「複合型」を学びましょう。

- スカラー型: 整数、浮動小数点数、論理値、文字
- 複合型: タプル型、配列

となっています。

**演習**

- [リンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=5eff8a75ba17095a08c0dcbb9920b3eb) を開いて、それぞれのデータ型を確認しましょう。
  - 参考: [TRPL: データ型](https://doc.rust-jp.rs/book-ja/ch03-02-data-types.html)
  - 参考: [The Rust Reference: Types](https://doc.rust-lang.org/reference/types.html)
- [問題へのリンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=5abae3a2b959cfbcb2b604b061dc6e50) を開いて、コンパイルが通るかどうか予想してみましょう。
  - なぜ `mut` をつけていないのにコンパイルが通るのでしょうか？
  - 参考: [スコープとシャドーイング](https://doc.rust-jp.rs/rust-by-example-ja/variable_bindings/scope.html)
  - [解答へのリンクはこちら](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=4d3643102387a5360ebe8c0a9a47cffd)
- [問題へのリンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=4ae72baee96801dcbbc44c195719d504) を開いて、コンパイルが通るように「ここを書き換えよ」というコメントのある行を書き換えよ。
  - なぜコンパイルが通らないのでしょうか？理由を考えてみましょう。
  - 参考: [Rust By Example: 型キャスティング](https://doc.rust-jp.rs/rust-by-example-ja/types/cast.html)
  - 参考: [keyword `as`](https://doc.rust-lang.org/std/keyword.as.html)
  - [解答へのリンクはこちら](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=2512e68e8c35da8d12c952006741c41a)

## 関数



## コメント

## フロー制御

## 所有権


