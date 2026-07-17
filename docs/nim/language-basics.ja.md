# 基本構文とステートメント

Nimはインデントでブロックを表します。Python風に見える部分がありますが、`proc`、`type`、`object`などはPascal/Ada系の影響も持ちます。

## 値、変更、定数

```nim
let name = "Nim"       # 再代入しない束縛
var count = 0           # 変更可能な束縛
const bufferSize = 4096 # コンパイル時定数

count += 1
echo name, ": ", count
```

基本は`let`、状態更新が必要なときだけ`var`、コンパイル時に確定する値には`const`を使います。型は多くの場合に推論されますが、API境界では明示した方が読みやすく安全です。

## プロシージャと戻り値

```nim
proc greet(name: string): string =
  "Hello, " & name & "!"

proc maximum(left, right: int): int =
  if left > right: left else: right

echo greet("Nim")
echo maximum(3, 7)
```

`proc`でプロシージャを定義します。最後の式を戻り値にでき、`result`へ代入して戻すこともできます。引数はセミコロンでグループ化できます。

```nim
proc contains(text: string; needle: char): bool =
  needle in text
```

## 条件分岐と繰り返し

```nim
let value = -2

if value > 0:
  echo "positive"
elif value < 0:
  echo "negative"
else:
  echo "zero"

for number in 1..5:
  if number mod 2 == 0:
    echo number, " is even"

var index = 0
while index < 3:
  echo index
  inc index
```

`if`は文としてだけでなく式としても使えます。`for`は範囲やイテレータを処理し、`1..5`は終端を含み、`0..<5`は終端を含みません。`while`、`break`、`continue`も通常の制御構文として利用できます。

## 型とデータ

```nim
type
  Status = enum pending, running, completed

  Article = object
    title: string
    tags: seq[string]

let article = Article(title: "Nim", tags: @["language", "native"])
echo article.title
```

`enum`は列挙、`object`は構造化データ、`seq[T]`は可変長シーケンスです。`@[]`はシーケンスリテラルです。型の詳細は[型システムとメモリ管理](types-and-memory.md)で扱います。

## モジュール

別ファイルの公開シンボルは末尾の`*`で公開し、利用側で`import`します。

```nim
# mathutil.nim
proc double*(value: int): int = value * 2

# main.nim
import mathutil
echo double(21)
```

標準ライブラリは`import std/[strutils, sequtils]`のように読み込みます。プラットフォーム差は`when defined(js)`の条件コンパイルで分けられます。

## 次へ

小さなCLIやライブラリを作ったら、[型システムとメモリ管理](types-and-memory.md)、[コンパイル時プログラミング](expressiveness-and-macros.md)、[バックエンドとFFI](compilation-and-ffi.md)へ進みます。正確な構文とバージョン固有の意味論は[公式マニュアル](https://nim-lang.org/docs/manual.html)で確認してください。
