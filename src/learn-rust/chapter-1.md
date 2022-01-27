# Rustを学ぼう

この文書は、[The Rust Programming Language](https://doc.rust-lang.org/book/) をこれから読む人に向けて書かれています。TRPLを読めば分かることがほとんどですが、いきなりTRPLを読むのは難しいという人向けにクッションがあったらいいのかもしれないという気持ちで書いています。

## 目次

<!-- toc -->

## Rustをインストールする前に、少し触ってみよう

Rustをインストールするのはとても簡単ですが、まずは気軽に試すためにブラウザだけで簡単なコードが試せる [Rust Playground](https://play.rust-lang.org/) を使ってみましょう。

初めて使う方は以下のようなコードが表示されていると思います。

```rs
fn main() {
    println!("Hello, world!");
}
```

これがRustのハローワールドです。main関数内に、標準出力を行うマクロ `println!` がありますね。マクロとは、他のコードを記述するためのコードを書く手段です。どのように他のコードを記述しているのかは [`println!` のドキュメント](https://doc.rust-lang.org/std/macro.println.html) から、`Macro std::println` の右側にある [`src`](https://doc.rust-lang.org/src/std/macros.rs.html#94-99) 部分を見てみると良いでしょう。

しかし現段階での理解は難しいと思うので、今は、関数は `!` なしで `func_name()` のように呼び出され、マクロは `!` ありで `macro_name!()` のように呼び出されるということを覚えていれば大丈夫です。

<!-- TODO: もう少し例を追加する、実行について述べる、所有権の例を出してコンパイルエラーに慣れさせる -->

**確認テスト**

- [ ] Rust Playgroundの使い方(コードの書き方、実行の仕方)を理解した
- [ ] 関数とマクロの違いをなんとなく理解した

## Cで書かれたコード例を通して、Rustの特徴を理解する
