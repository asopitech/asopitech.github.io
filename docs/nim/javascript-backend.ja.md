# JavaScriptバックエンド

## 結論：WASMではなく、NimからJavaScriptを直接生成する

NimのJavaScriptバックエンドは、NimコードをWASMへ変換する機能ではありません。型検査、マクロ展開、ジェネリクス具体化を行ったNimの内部表現から、**JavaScriptソースを直接生成**します。

```text
Nimソース → 型検査・マクロ展開・ジェネリクス具体化 → JavaScriptソース
         → ブラウザ / Node.js / Electron
```

そのため線形メモリやWASM FFI境界を挟まず、JavaScriptのオブジェクト、文字列、Promise、DOM APIを比較的直接的に扱えます。一方、ネイティブ版と同一の実行モデルではありません。OS固有処理、手動メモリ管理、生ポインタ、スレッドなどは使えず、**Nimを型付きのJavaScript開発言語として使うバックエンド**と理解するのが正確です。

## 基本的な使い方

```nim
proc greet(name: string): string =
  "Hello, " & name

echo greet("Nim")
```

```bash
nim js app.nim
nim js -r app.nim
nim js -d:release -o:public/app.js src/app.nim
```

通常は`app.js`が生成され、`-r`はNode.jsでの実行まで行います。Nimコンパイラは高度なバンドラーやminifierではないため、縮小、tree shaking、source map、npm依存統合にはesbuild、Rollup、Viteなどを後段で組み合わせます。

## 出力と型検査

NimモジュールをES Modules群へ一対一に変換するのではなく、必要なコードをまとめた長い単一`.js`成果物を生成する設計です。アプリ全体、Web Worker、HTMLから直接読むスクリプト、Node.js用の単一プログラムには適します。一方、既存TypeScriptプロジェクトへ細かいESMモジュールとして混ぜるにはラッパーが必要です。

出力はJavaScriptでも、入力は通常のNimとして型検査されます。静的型、型推論、enum、`distinct`、variant object、ジェネリクス、concept、オーバーロード、テンプレート、マクロを利用できます。マクロとジェネリクスはJavaScript生成前に処理されます。

```nim
template repeat(count: int, body: untyped) =
  for _ in 0..<count:
    body

repeat 3:
  echo "hello"
```

## JavaScriptとのFFI

`importjs`でJavaScript式を型付きNim宣言として包めます。`#`は引数挿入位置です。

```nim
proc jsMax(a, b: float): float
  {.importjs: "Math.max(#, #)".}

proc toUpperCase(value: cstring): cstring
  {.importjs: "#.toUpperCase()".}
```

グローバルを取り込むには`importc`と`nodecl`を使えます。

```nim
import std/jsffi

var window {.importc, nodecl.}: JsObject
var document {.importc, nodecl.}: JsObject
```

Nim関数を公開するには`exportc`を使いますが、公開ライブラリでは薄いJavaScript/TypeScriptファサードを用意し、名前mangling、単一JS出力、ESM/CommonJS、TypeScript定義をそこで吸収する方が保守しやすいです。

## `jsffi`、DOM、Fetch、Promise

`std/jsffi`は中心的なFFIモジュールです。`JsObject`は動的JSオブジェクト、`JsAssoc[K, V]`は型付き連想オブジェクトを表します。`toJs`と`to(T)`は境界上の値を型付けして扱うための機能であり、外部データの安全なデシリアライズを自動で保証するものではありません。`jsNull`と`jsUndefined`は別物として扱います。

```nim
import std/jsffi

let obj = newJsObject()
obj.name = "Alice"
let jsValue = 42.toJs
let value = jsValue.to(int)
```

ブラウザでは`std/dom`、非同期処理ではネイティブ用`asyncdispatch`ではなく`std/asyncjs`、HTTPでは`std/jsfetch`を使います。

```nim
import std/[asyncjs, jsfetch]

proc loadText(url: cstring): Future[cstring] {.async.} =
  let response = await fetch(url)
  if not response.ok:
    raise newException(IOError, "HTTP error: " & $response.status)
  return await response.text()
```

`Future[T]`はPromiseに対応します。実際の最低ECMAScriptレベルは使う機能に依存し、`asyncjs`やBigIntを使うなら現代的なブラウザ／Node.jsを対象にします。CORS、credentials、cache、redirectはJavaScriptのFetch APIの挙動に従います。

## Node.jsとnpm

Node.jsも対象にできますが、Nimの`os`、`files`、`osproc`がNode APIへ自動変換されるわけではありません。CommonJSなら`require`、または`importjs`で明示的にNode APIを包みます。

```nim
import std/jsffi
let fs = require("fs")
```

現代的なVite/Rollupプロジェクトでは、npmライブラリを読み込むJavaScriptラッパーを置き、`globalThis`経由などでNimへ狭いAPIを渡す方式が安定します。NimコンパイラがTypeScript型定義やnpm package exportsを自動解釈するわけではないため、Nim側のバインディングは必要です。

## 数値・Unicode・メモリの注意点

JavaScriptの通常数値は倍精度浮動小数点です。64ビット整数、大きなID、ビット演算、暗号、オーバーフロー依存アルゴリズム、バイナリ形式はネイティブ版と同じ機械整数モデルを前提にせず検証します。正確な大整数には`std/jsbigints`の`JsBigInt`、文字列、32ビット値二つ、TypedArrayなどの明示的表現を選びます。

Nimの文字列APIにはUTF-8バイト列を前提とするものがある一方、JavaScript文字列はUTF-16コード単位です。`len`、インデックス、絵文字、サロゲートペア、バイト処理はUnicodeを含む両バックエンドテストを行ってください。

JSバックエンドの`ref object`はJavaScriptオブジェクトとなり、JS GCに管理されます。`alloc`、`dealloc`、`zeroMem`、生ポインタ、任意のunsafe cast、C構造体と同一のレイアウト、デストラクタによるネイティブリソース解放には依存できません。socket、reader、URLなどの外部リソースは対応するJavaScript APIで明示的に閉じます。

## 向く用途とWASMとの使い分け

パーサー、バリデータ、ルールエンジン、状態機械、数式・テキスト変換、シリアライザー、DSL、DOM/Canvas UI、Fetchクライアント、Web Worker、Electron、Node.jsの変換ツールには適性があります。

| 観点 | Nim JS | Nim→C→WASM |
| --- | --- | --- |
| DOM・Fetch・Promise | 直接的 | JS bridgeが必要 |
| JS FFI・文字列・オブジェクト | 容易 | 変換・コピーが必要 |
| Cライブラリ | 原則利用不可 | Emscripten対応なら可能 |
| ポインタ・バイナリ・SIMD | 不向き | 比較的向く |
| GPU・OS・ネイティブスレッド | 不向き | 別のネイティブ経路を検討 |

DOM・Fetch・Promise・npm連携、パーサー・業務ロジック・文字列処理が中心ならJSバックエンドを先に検討します。C/C++ライブラリ、厳密なメモリ配置、大量の数値・SIMD・バイナリ処理が重要ならWASMを比較します。

## なぜNimはJavaScriptを厚くサポートするのか

Nimの中心はCではなく、型検査・マクロ展開後のNim ASTです。C/C++/Objective-CとJavaScriptは、その内部表現の主要な出力先です。JSバックエンドは後付けの簡易トランスパイラではありません。

同じ言語でブラウザクライアントとネイティブサーバーを扱い、型、バリデーション、パーサー、プロトコル、ビジネスルールを共有することが重要な動機でした。JavaScriptはブラウザ、Node.js、Electron、WebViewなどで動くポータブルな実行環境でもあります。NimのGC参照オブジェクト、クロージャ、例外、動的配列、文字列、ジェネリクス具体化、マクロ展開済みコードはJSへ比較的自然に落とせます。

Nim JSの本質は、TypeScriptや最新フロントエンドツールチェーンを全面置換することではなく、**Nimの静的型、ジェネリクス、マクロ、パーサー記述力をJavaScript実行環境へ持ち込むこと**です。
