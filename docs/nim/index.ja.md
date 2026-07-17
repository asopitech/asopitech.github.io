# Nim：Cの地面を捨てずに、高水準に書く

Nimは単なる「Python風の構文を持つ高速言語」ではありません。目指しているのは、**Cに近い実行性能・配置自由度・FFI能力を維持しながら、静的型付け、型推論、抽象化、コンパイル時メタプログラミングによって、スクリプト言語に近い開発速度を得ること**です。

より正確には、Nimは「Cを厳格に安全化した言語」ではなく、**C/Pascal系のシステムプログラミングを、Pythonのような記述感と強いコンパイル時機能で再設計した言語**です。Rustのように危険な操作を型システムで原則として表現不能にする設計ではありません。通常は高水準に書け、必要なときだけポインタ、C ABI、手動メモリ管理、インラインCへ降りられます。

## このガイドの構成

このサイトは公式リファレンスを複製するのではなく、サイトマップに沿ってNimの設計・内部・実践を段階的に理解するための **Nim Architecture Guide** です。各セクションはサイドバーでも階層表示されます。

1. **はじめに** — [概要](index.ja.md)、[このガイドの方針](guide-scope.md)、[設計思想と安全性](design-and-safety.md)。
2. **チュートリアル** — [セットアップとビルド](getting-started.md)、[基本構文とステートメント](language-basics.md)。
3. **言語全体像** — [言語アーキテクチャ](architecture.md)。
4. **バックエンドとFFI** — [C/C++との相互運用](compilation-and-ffi.md)、[JavaScript](javascript-backend.md)、[WASMとMoonBit](wasm-and-moonbit.md)。
5. **型・メモリ・コンパイル時機能** — [型システムとORC/ARC](types-and-memory.md)、[マクロとメタプログラミング](expressiveness-and-macros.md)。
6. **ランタイム・Web・GUI・AI** — [非同期・NimScript](runtime-and-tooling.md)、[Nim製JSエンジン](javascript-engines.md)、[GUI開発](gui.md)、[GPUとLLM](gpu-and-llm.md)。
7. **比較と実践** — [C/Rust/Zig比較](comparisons.md)、[用途・弱点・採用判断](adoption.md)。

未公開のチュートリアル、標準ライブラリ、GUI、システム、エコシステム、ソース読解、ロードマップ、付録は、[このガイドの方針](guide-scope.md)に従い段階的に追加します。

## Efficient, Expressive, Elegant

Nimが掲げる価値は **Efficient / Expressive / Elegant** です。低レベル性能、高水準構文、コンパイル時プログラミングを別々の層ではなく、同じ言語で連続的に扱える点に特徴があります。VMに依存しないネイティブ実行ファイル、値型とスタック配置の活用、デストラクタとmove semantics、C/C++/JavaScriptバックエンド、ASTを直接操作できるマクロが、その土台です。

## 公式リソース

仕様・実装依存の挙動は、必ず一次情報で確認してください。

- [Nim公式サイト](https://nim-lang.org/)
- [言語マニュアル](https://nim-lang.org/docs/manual.html)
- [チュートリアル](https://nim-lang.org/docs/tut1.html)
- [標準ライブラリ](https://nim-lang.org/docs/lib.html)
- [Nimble](https://github.com/nim-lang/nimble)
