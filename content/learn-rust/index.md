+++
title = "Rustを学ぼう"
date = "2022-02-06T01:35:35+09:00"
draft = false
+++

# Rustを学ぼう

この資料は、 [The Rust Programming Language](https://doc.rust-lang.org/book/) (TRPL) を参考に、Rustはいいぞ！ということを伝えるために作った資料です。TRPLの1~3章に相当するので、本資料を読んだ後にTRPLの該当部分を読んでみるとスラスラ読めると思います。

## はじめに

初めてのプログラミング言語を学ぶときには、「その言語の良さが分からないと学ぶモチベーションが上がらない」でも、「言語の良さをきちんと知るにはその言語を少しは触る必要がある」というジレンマがあります。

そこで今回は、`if`や`for`などの基本的な部分+所有権というRustの特徴的な部分をざっと紹介した後で、Rustのよさである「安全性」「実行速度」「開発体験」をそれぞれ手を動かしながら体験してもらうという構成にしました。

つまり、先に簡単にRustの勉強をした後で、Rustの良さを知る、という流れです。

それでは始めていきましょう。

## ブラウザからRustを気軽に試す

Rustをインストールするのはとても簡単ですが、まずは気軽に試すためにブラウザだけで簡単なコードが試せる [Rust Playground](https://play.rust-lang.org/) を使ってみましょう。このPlaygroundはブラウザで入力したコードを裏でサーバに投げてコンパイルして実行して結果を返してくれるものです。

初めて使う方は以下のようなコードが表示されていると思います。この部分は自由に編集することができます。

```rust
fn main() {
    println!("Hello, world!");
}
```

これがRustのハローワールドです。main関数内に、標準出力を行うマクロ `println!` がありますね。マクロとは、他のコードを記述するためのコードを書く手段です。どのように他のコードを記述しているのかは [`println!` のドキュメント](https://doc.rust-lang.org/std/macro.println.html) から、`Macro std::println` の右側にある [`src`](https://doc.rust-lang.org/src/std/macros.rs.html#94-99) 部分を見てみると良いでしょう。

しかし現段階での理解は難しいと思うので、今は、関数は `!` なしで `func_name()` のように呼び出され、マクロは `!` ありで `macro_name!()` のように呼び出されるということを覚えていれば大丈夫です。

それではこのコードを実行してみましょう。左上の「`RUN`」ボタンを押してみてください。画面下半分のところに「`Progress`」で黒丸が表示され、少し時間が経つと「`Standard Error`」, 「`Standard Output`」部分に表示がされていると思います。「`Standard Error`」が標準エラー出力、「`Standard Output`」が標準出力です。

また、「`SHARE`」から「`Permalink to the playground`」をクリックすると、 [https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=7c4f223819334660c7915edb9830a15e](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=7c4f223819334660c7915edb9830a15e) のように書いたコードへのリンクが得られます。

使い方はこれで以上です。それでは、どの言語にも共通する`if`や`for`のような基本的な事柄をPlayground上で学んでみましょう。

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

```none
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
- [問題へのリンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=343546d1d71252ab2ee06a09fcbc2741) を開いて、コンパイルが通るかどうか予想してみましょう。
  - なぜ `mut` をつけていないのにコンパイルが通るのでしょうか？
  - 参考: [スコープとシャドーイング](https://doc.rust-jp.rs/rust-by-example-ja/variable_bindings/scope.html)
  - [解答へのリンクはこちら](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=6d1cd12e4ef538dd1094e1daa6b3d49e)
- [問題へのリンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=4ae72baee96801dcbbc44c195719d504) を開いて、コンパイルが通るように「ここを書き換えよ」というコメントのある行を書き換えよ。
  - なぜコンパイルが通らないのでしょうか？理由を考えてみましょう。
  - 参考: [Rust By Example: 型キャスティング](https://doc.rust-jp.rs/rust-by-example-ja/types/cast.html)
  - 参考: [keyword `as`](https://doc.rust-lang.org/std/keyword.as.html)
  - [解答へのリンクはこちら](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=2512e68e8c35da8d12c952006741c41a)

## 関数

関数は以下のように書きます。関数は値を受け取って処理を行うものなので、引数と返り値に注目すると分かりやすいと思います。

```rust
// 関数
fn function_1() {
    println!("Number = {}", 123);
}

// 引数のある関数
fn function_2(x: i32) {
    println!("Number = {}", x);
}

// 返り値のある関数
fn function_3(x: i32) -> i32 {
    x + 1
}
```

引数はカッコの中に、 `func_name(x: Ty)` のように変数 `x` の型 `Ty` をコロン `:` を間に挟んで書きます。

返り値は、`func_name() -> Ty` のように返り値の型 `Ty` を `->` の後ろに書きます。

**演習**

- [問題へのリンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=0d0e85e7fcad3de7baebceb9bf4dcd89) を開いて、コンパイルが通るように関数を書き加えてみましょう。
  - 上の `function_3` を真似してみましょう。
  - 参考: [TRPL: 関数](https://doc.rust-jp.rs/book-ja/ch03-03-how-functions-work.html)
  - [解答へのリンクはこちら](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=0b0119736025db8697b879ef775f64ed)

## コメント

コメントは `// (comment)` や `/* (comment) */` と書くことができます。

```rust
// hello, world 一行のコメント

/* ok
This is comment
複数行書けるコメント
 */
```

## フロー制御

ここでは、 `if` 式とループを学びます。

### 前提知識: 式と文

プログラムにおいて、式と文は区別されます。式は値を返すもの、文は値を返さないものです。

Rustでは、以下のように多くのものが式です。

```rust
let x = 5; // 全体で見るとlet statement(文)
	
if true {2} else {5} // これは値を返す(式)
10 // これは値を返す(式)
{
	5
} // これは値を返す(式)

{
	println!("hello");
} // 実はこれも値を返す。セミコロンで終わりリターンのないコードブロックは `()` を返す。
```

### if式とループ

`if` 式は、論理値を用いて条件分岐するときに使います。

```rust
let x = 5;

// 論理値を入れる
if x != 0 {
    println!("x is not 0.");
}

// elseを使って2つに分岐
if x != 0 {
    println!("x is not 0.");
} else {
    println!("x is 0.");
}

// else if を使って複数に分岐
if x > 0 {
    println!("x is +.");
} else if x < 0 {
    println!("x is -.");
} else {
    println!("x is 0.");
}

// ifは式なので、値を返却できる
let y = if x > 0 {
    10 // 値を返却するために `;` がつかないことに注意
} else {
    -10 // let variable = if cond { A } else { B }; の時、AとBは同じ型
};
```

ループは、`loop`, `while`, `for` の3つが使えます。

```rust
// loop: プログラムをCtrl+cで止めるまで永遠にループする
loop {
    println!("Hello!");
}

// while: 条件式が`true`の間だけループする
let mut x = 5;
while x > 0 {
    println!("{}", x);
    x = x - 1;
}

// for: 決められた固定回数だけループする。添字を使うループとして、配列を対象によく使われる
for i in 0..10 {
    println!("{}", i);
}
```

`if` も `for` も、条件式やループの添字部分を `()` で囲うことがないのが特徴的ですね。

**演習**

- [問題へのリンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=9fdf6ee8033955d7479699a185334346) を開いて、コンパイルが通るように「ここを書き換えよ」というコメントのある行を書き換えよ。
  - なぜコンパイルが通らないのでしょうか？
  - 参考: [TRPL: フロー制御](https://doc.rust-jp.rs/book-ja/ch03-05-control-flow.html)
  - [解答へのリンクはこちら](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=5fa34d7023b8607beac73d32430d16a3)
- [リンク](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=5c1f1ecf8fbbc73ea9d62adc8635c52b) を見て、for文の使い方を学びましょう。
  - 参考: [Rust By Example: forループ](https://doc.rust-jp.rs/rust-by-example-ja/flow_control/for.html)
  - 参考: [keyword `for`](https://doc.rust-lang.org/std/keyword.for.html)
  - 参考: [`rev()`](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.rev)
  - 参考: [`iter()`](https://doc.rust-lang.org/std/primitive.slice.html#method.iter)
  - Rustにはたくさんの便利なメソッドがあるので、やりたいことがあれば標準ライブラリのドキュメントを検索してみると良いでしょう。

## 所有権

これは別の記事で説明します。

→ [pub.docs - Rustの所有権を理解する](chapter-1-ownership/index.md)

## 終わりに

TRPLは素晴らしい資料なので、本資料も工夫したかったのですが難しかったです。この資料を読んで、TRPL読んでみようかな、と思ってもらえたら嬉しいです。
