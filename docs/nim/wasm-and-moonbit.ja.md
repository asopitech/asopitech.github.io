# WebAssemblyとMoonBitの比較

## 結論

**WASMを主目的にするならMoonBitが適しています。** 一方、C/C++資産との統合、ネイティブ実行、既存コードの再利用まで含めるならNimの方が広い選択肢です。NimはC/C++変換を主軸とし、通常はEmscriptenなどを経由してWASMへ持ち込みます。MoonBitは`wasm`と`wasm-gc`を第一級の直接ターゲットとして扱います。

| 観点 | Nim | MoonBit |
| --- | --- | --- |
| WASMの位置付け | C/C++ツールチェーン経由の配布先 | 中心的ターゲット |
| 生成経路 | Nim → C/C++ → Emscripten → WASM | MoonBit → WASM |
| WASM GC | 直接の主ターゲットではない | 公式バックエンド |
| C/C++連携 | 非常に強い | native/Cでは可能、WASMではホストFFI中心 |
| ネイティブ用途 | 強い | 対応するがWASMほど主戦場ではない |
| ASTマクロ | 非常に強い | NimほどASTマクロ中心ではない |

## コンパイルとメモリ境界

NimのWASM化はCバックエンドの移植性を活用します。生成CにNimランタイム、メモリ管理、例外・文字列処理、Emscriptenランタイム、JS glue、libc互換層が関わるため、実際のサイズと初期化時間は測定が必要です。

MoonBitの通常`wasm`はlinear memoryを使い、`wasm-gc`はWASMの`struct`、`array`、GC参照型を活用します。WASM GCはブラウザ・JSとの文字列やオブジェクト境界を高水準に扱える可能性がありますが、ホストとの完全な透過共有を保証するものではなく、FFI設計は依然必要です。

NimのWASM境界では一般にpointer + lengthを使います。

```nim
proc parseSql(input: ptr char, length: cint): ParseResult
  {.exportc, cdecl.}
```

JS文字列をWASM linear memoryへコピーし、結果をシリアライズして返す設計では、文字列コピーとAST返却がボトルネックになり得ます。

## FFIとブラウザ統合

Nimの最大の強みはC ABIです。C、C++、Rustの`extern "C"`、DuckDB、SQLite、ICU、RE2、Arrow C Data Interfaceのような既存資産と結び付けやすく、ネイティブのRustエンジンとNimパーサーをC ABIで接続する構成は自然です。

DOM操作中心なら、Nimには`nim js app.nim`という直接JavaScript出力の選択肢もあります。MoonBitは`js`、`wasm`、`wasm-gc`を同じツールチェーンで扱いやすい反面、WASM側の外部環境とのやり取りはホスト関数に依存します。

## SQLパーサーでの判断

既存Nim実装を維持する根拠は、ネイティブ版とWASM版の共通化、RustとのC ABI、Nimマクロ、C/C++ライブラリ利用、既存コードの安定性、WASMが複数ターゲットの一つであることです。

ブラウザWASMが主力で、小さいバイナリ・起動時間・JS文字列交換・WASM GC・独立コンポーネントが重要なら、MoonBitの比較実装を検討できます。ただし二重実装は文法差分を生みます。

推奨は、まずNim版をWASM化して `.wasm` サイズ、初期化時間、1文あたりのパース時間、100万文のthroughput、JS→WASM転送、ピークメモリ、AST返却、エラーメッセージ品質を測ることです。実害が確認できた場合に、tokenizer、式パーサー、単純なSELECT、AST JSON出力だけをMoonBitで比較実装します。
