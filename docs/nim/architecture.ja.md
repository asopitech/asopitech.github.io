# 言語アーキテクチャ

NimをCの構文糖衣ではなく複数バックエンドを持つ言語として理解するには、ソースから成果物までの段階を分けて見ることが重要です。

```text
Nim source
  ↓ parser
AST
  ↓ semantic analysis / type checking
型付けされた内部表現
  ↓ macro expansion / generic instantiation / compile-time evaluation
正規化されたプログラム
  ↓ backend
C / C++ / Objective-C / JavaScript
```

C/C++系ではさらに対象プラットフォームのコンパイラがネイティブ実行ファイルやライブラリを作ります。JavaScriptバックエンドはJavaScriptソースを直接生成します。マクロとジェネリクスは出力先へ渡る前に処理されるため、各バックエンドはNimの高水準機能を実装し直す必要がありません。

## 言語レベルとバックエンド固有機能

| 言語レベル | バックエンド固有 |
| --- | --- |
| 型、制御構文、object、ジェネリクス、マクロ、例外 | ポインタ、C ABI、DOM、Promise、OS API、JS GC |

この境界を意識すると、共有可能なロジックと、`when defined(js)`やFFIで分離すべきロジックを判断しやすくなります。コンパイラやNIF/Nimonyなどの将来の内部表現は、[ロードマップとガイドの範囲](guide-scope.md)で追跡します。
