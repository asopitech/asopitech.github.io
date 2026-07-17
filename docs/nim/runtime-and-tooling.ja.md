# エラー処理・並行処理・NimScript

## 例外と効果

Nimでは例外が標準的なエラー処理の一つです。

```nim
try:
  echo parseInt("abc")
except ValueError as error:
  echo error.msg

proc parseValue(text: string): int {.raises: [ValueError].} =
  parseInt(text)
```

`raises`プラグマで投げ得る例外を制限できます。例外、IOなどの副作用タグ、ロックレベル、GC安全性を記述・検査する効果システムもあります。一方、言語全体がRustの`Result[T, E]`のような代数的エラー型を中心に作られているわけではありません。

## 非同期・スレッド

Nimはスレッド、非同期IO、チャネル、スレッドプールを扱えます。

```nim
import std/[asyncdispatch, asyncnet]

proc main() {.async.} =
  let socket = newAsyncSocket()
  await socket.connect("example.com", Port(80))

waitFor main()
```

`async`/`await`はコンパイル時変換を活用します。ARC/ORCは非atomic reference countingを中心とするため、参照型をスレッド間で自由に共有する前提ではありません。所有権、メッセージパッシング、スレッドローカルなデータ構造を意識して設計します。

## NimScript

NimScriptはビルド、テスト、デプロイ、ベンチマーク、ドキュメント生成などに使えるスクリプト実行形態です。

```nim
# build.nims
task build, "Build the application":
  exec "nim c -d:release src/app.nim"

task test, "Run tests":
  exec "nimble test"
```

ネイティブアプリケーション、ライブラリ、コンパイル時処理、ビルドスクリプトを、近い言語感覚で扱えることがNimの強みです。
