# コンパイルとC相互運用

## コンパイルの流れ

典型的には、NimコンパイラがCソースを生成し、GCC、Clang、MSVCなどのCコンパイラがネイティブ実行ファイルを生成します。

```text
Nimソース → Nimコンパイラ → Cソース → Cコンパイラ → ネイティブ実行ファイル
```

```bash
nim c app.nim
nim c -r app.nim
nim c -d:release app.nim
```

Cはポータブルな低レベル中間表現として使われます。型検査、ジェネリクスの具体化、マクロ展開、メモリ管理コードの挿入は、その前にNimコンパイラが行います。そのため、NimをCの薄い構文糖衣と捉えるのは正確ではありません。

## 複数バックエンド

Cに加え、C++、Objective-C、JavaScriptを主要な出力先として選べます。

```bash
nim cpp app.nim
nim objc app.nim
nim js app.nim
```

ターゲットを採用する前に、利用するパッケージ、生成物サイズ、既存ツールチェーンとの接続を検証してください。

## Cを呼ぶ・Cから呼ばれる

`importc` でC関数や変数を取り込み、`exportc` でNimの関数をC ABIに公開できます。

```nim
proc printf(format: cstring): cint
  {.importc, header: "<stdio.h>", varargs.}

proc add(a, b: cint): cint {.exportc, cdecl, dynlib.} =
  a + b
```

共有ライブラリ境界では、Nim固有の`string`、`seq`、`ref object`を直接公開せず、固定幅整数、ポインタ＋長さ、C互換struct、opaque handle、明示的な生成・破棄関数、エラーコードに限定すると安定します。詳細は[言語マニュアル](https://nim-lang.org/docs/manual.html)を参照してください。
