# JavaScriptエンジンをNimで実装する

## Bali・Sev・Ferus

Nimで実装されたJavaScript実行系として、**Bali** と、その上にホスト機能を加える **Sev** があります。Baliは字句解析、構文解析、ECMAScript実行系をNimで実装し、ECMAScript仕様への準拠を目指すJavaScriptエンジンです。概念的な流れは次のとおりです。

```text
JavaScript source → Bali lexer → parser → AST / IR → Nim interpreter
```

SevはBaliを実行エンジンとして使うJavaScriptランタイムです。V8とNode.jsの関係にたとえるなら、Baliが言語エンジン、SevがI/OやホストAPIを加える側に相当します。ただし規模、互換性、実用性はNode.jsとは比較にならず、実験的プロジェクトです。

FerusはHTML、DOM、CSS/layout、描画、JavaScript実行を統合しようとするNim製の小規模Webエンジンで、JavaScript部分にBaliを使います。MirageはBali周辺で開発されているIR／バイトコード処理系で、AST逐次評価からより構造化された実行へ進む方向です。

## 現時点での評価

| 項目 | Bali / Sev |
| --- | --- |
| Nimだけで実装されたJSエンジン | はい |
| ECMAScript準拠検証 | Test262を追跡 |
| Node.js API・npm互換 | ほぼない |
| JIT・成熟した高速VM | ない／開発途上 |
| async/event loop | Sev側で開発途上 |
| 本番ランタイム用途 | 不適 |
| 言語処理系・Webエンジン研究 | 有望 |

結論として、Nim製JSランタイムは存在しますが、QuickJSやNode.jsの代替として使える段階ではありません。価値は「すぐに使える最速ランタイム」ではなく、JavaScript処理系、独自ホストAPI、サンドボックス、DSL的サブセット、WebエンジンをNimで制御・改造できることです。

本番のNimアプリにJavaScript実行を埋め込むなら、C APIを持つQuickJSやDuktape、あるいはJavaScriptCore、V8、Rust製BoaをFFIで利用する方が現実的です。

## Rust製Boaとの比較

BoaはRustアプリケーションへJavaScriptを埋め込むことを主目的にしたエンジンで、Parser、AST、bytecode compiler、register VM、独自GC、runtimeをcrate単位で分離しています。Test262準拠、モジュール、国際化などの成熟度と埋め込みAPIでは、現時点でBaliより先行しています。

| 観点 | Bali | Boa |
| --- | --- | --- |
| 主な目的 | Nim製Web/JS処理系の研究 | Rustへ埋め込むJSエンジン |
| 実行系 | InterpreterとMirage開発 | Bytecode compilerとregister VM |
| GC | Nim側の管理に依存 | 専用GC |
| 埋め込みAPI | 発展途上 | crateとして整備 |
| 実用成熟度 | 実験的 | より成熟している |

目的は完全には同じではありません。BaliはNim製ブラウザスタックという独自性を持ち、Boaは組み込みエンジンとしてのAPI・モジュール性・準拠度を優先しています。

## Boa、Nova、Denoを混同しない

Rust界隈では、Boa、Nova、Denoを同じ「Rust製JavaScript」として扱わないことが重要です。

| プロジェクト | 種類 | 位置付け |
| --- | --- | --- |
| Boa | Rust製JavaScriptエンジン | 埋め込み用途で最も成熟した選択肢の一つ |
| Nova | Rust製JavaScript + WASMエンジン | 発展途上の実装 |
| Deno | JavaScriptランタイム | ランタイムはRustだがエンジンはC++製V8 |

Denoを「Rust製JSエンジン」と呼ぶのは正確ではありません。Rustなのはランタイム側で、JavaScriptの実行エンジン自体はV8です。

## Boaのアーキテクチャ

Boaは、ASTを直接評価する段階からbytecode compilerとregister VMを中心にした構成へ発展しています。

```text
JavaScript → lexer → parser → AST → bytecode compiler → register VM → GC
```

AST、parser、engine、GC、runtimeはcrateとして分離され、RustアプリケーションへJavaScriptを埋め込むAPIを意識して設計されています。概念的には`Context`、`Source`、`JsValue`を使ってJavaScriptを評価する構成です。

Test262準拠、Temporal、Intl、moduleなどの対応を重視し、専用GC、VM、runtimeを持つ点がBaliとの大きな差です。ただしBoaもV8級の性能・互換性を目標達成済みとみなすべきではなく、用途ごとに公式の対応状況とテストを確認してください。

## 実行モデル・GC・ランタイム

| 観点 | Bali / Sev | Boa |
| --- | --- | --- |
| 主な実行モデル | AST interpreter、Mirageを開発 | bytecode compiler、register VM |
| メモリ管理 | NimのARC/ORCを基盤にする設計 | `boa_gc`による専用GC |
| ランタイム | SevがホストAPI・I/Oを担う | `boa_runtime`が周辺APIを整備中 |
| Node.js互換 | ない | Node.jsそのものではない |
| npm互換 | ない | Node/npm互換を前提にしない |

BoaはJavaScriptエンジンであり、Node.jsではありません。`fetch`、`queueMicrotask`、`setTimeout`などの周辺機能が整備されても、Node互換ランタイムと同一視しないようにします。Bali/Sevも同様に、エンジンとランタイムを分けた研究・開発段階の構成です。

## 目的の違いから選ぶ

Boaは「RustアプリケーションにJavaScriptを埋め込む」ために、API、モジュール性、実行系、準拠度を磨いています。Baliは「NimだけでJavaScript処理系やWebブラウザを実装する」方向性が強く、Ferusとの統合や処理系研究に価値があります。

したがって、成熟度・ECMAScript準拠・埋め込みエコシステムを優先するならBoaが先行します。Nimの言語処理、独自ホストAPI、サンドボックス、Webエンジン、DSL的なJSサブセットを研究したいなら、Bali/Sevは興味深い題材です。Nimアプリに本番のスクリプト実行を組み込む判断では、QuickJS/DuktapeをC FFIで利用する方法と、Rust側にBoaを置きC ABIやRPCで接続する方法を比較してください。
