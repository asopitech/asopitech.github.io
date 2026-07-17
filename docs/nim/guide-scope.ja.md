# Nim Architecture Guide：このサイトの方針

この特設サイトは公式リファレンスの置き換えではなく、**Nimを理解するためのArchitecture Guide**です。公式マニュアル、ブログ、フォーラム、GitHub Issueに散らばりやすい「なぜこの設計なのか」「どの用途で何を選ぶべきか」を、設計思想と実践的な比較を軸に整理します。

## 5つのシリーズ

- **Why Nim?** — 設計思想、安全性、C ABI、ARC/ORC、マクロ、複数バックエンド。
- **Inside Nim** — コンパイラ、AST、意味解析、ジェネリクス、ランタイム、標準ライブラリ。
- **Nim vs X** — C/C++、Rust、Zig、Go、D、MoonBit、TypeScriptとの比較。
- **Nim in Practice** — Web、GUI、システム、AI、データ処理、パーサー、DBなどの実践。
- **Nim Ecosystem Watch** — Nimble、LSP、Nimony、NIF、WASM、周辺ライブラリと動向。

## 全体像

```text
Nim source
  → parser / AST / semantic analysis
  → macro expansion / generic instantiation
  → backend (C / C++ / Objective-C / JavaScript)
```

この図の各段階と、バックエンド固有の機能・制約を結び付けて説明することが、このサイトの中心です。文法の完全な仕様は公式ドキュメントを優先し、本サイトでは設計上の理由、トレードオフ、実装・採用の判断材料を提供します。

## 今後の拡張

チュートリアル、型システム、メモリ管理、コンパイル時計算、FFI、標準ライブラリ、GUI、Web、システム、エコシステム、実践事例、ソースコード読解、ロードマップ、FAQ・用語集を段階的に追加します。最新性が重要な情報は一次情報へのリンクと更新日を明記し、断定より検証可能な比較を優先します。
