# 型システムとメモリ管理

## 型推論とAPI境界

ローカルでは型推論を使い、公開APIでは意図を明示できます。

```nim
let x = 10
let values = @[1, 2, 3]

proc calculate(values: openArray[float64]): float64 =
  for value in values:
    result += value
```

`let` は再代入不可、`var` は変更可能、`const` はコンパイル時定数です。`let` は不変束縛であり、Rustの所有権とは別の概念です。

## 値型を基本にする

`object` は基本的に値型です。

```nim
type Point = object
  x, y: float64

var a = Point(x: 1, y: 2)
var b = a
b.x = 100
echo a.x # 1
```

参照共有が必要なときは `ref object` を使います。この「基本は値、必要な場合だけ参照」という選択は、予測可能性と利便性を両立させます。

## ORC/ARCとデストラクタ

Nim 2系ではORCがデフォルトのメモリ管理戦略です。ORCは参照カウントを基礎に循環参照を扱い、ARC/ORCではコンパイラがデストラクタとmove semanticsを用いるコードを挿入します。古典的なstop-the-world GCとは異なり、解放タイミングは比較的決定的です。

```nim
type FileHandle = object
  handle: pointer

proc `=destroy`(x: FileHandle) =
  if x.handle != nil:
    closeNativeHandle(x.handle)
```

これはRustの借用検査とは異なります。循環グラフや参照型を扱いやすい一方、すべての参照関係を静的に証明するものではありません。
