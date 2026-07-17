# 設計思想と安全性

## 「型安全なC」はどこまで正しいか

NimはCより通常のコードで型を壊しにくく、異なる意味を持つ同一表現の値を区別できます。

```nim
type
  UserId = distinct int
  OrderId = distinct int

proc findUser(id: UserId) = discard
let orderId = OrderId(10)
# findUser(orderId) # コンパイルエラー
```

`distinct` は内部表現を変えずにドメイン型を作れます。range型、列挙型、配列長を含む型、判別共用体（variant object）も、Cの素朴な整数や`union`より強い構造を表現します。

```nim
type
  Percentage = range[0..100]
  ValueKind = enum vkInt, vkString
  Value = object
    case kind: ValueKind
    of vkInt: intValue: int
    of vkString: stringValue: string
```

## Rustと異なる安全性モデル

NimはRust式のborrow checkerを持ちません。`nil`、生ポインタ、`cast`、手動メモリ確保、外部Cコード、ランタイムチェックの無効化といった低レベルなescape hatchを利用できます。

```nim
var x = 10
let p = addr x
p[] = 20
echo x
```

したがってNimの安全性は「安全なサブセットを強制する」ものではなく、**通常は型付きで高水準な標準経路を使い、必要時には低レベルへ到達できる**というものです。RustよりD、Ada、Modern C++、Object Pascal、Zigに近い面がありますが、記述感はよりPython寄りです。

## null安全性について

`ref object` は `nil` になれます。通常のNimを、RustやKotlinのようにnull可能性が常に型で明示される言語とみなすのは正確ではありません。型安全性は有用ですが、低レベル機能や`nil`を含むため、Rust級の安全保証を期待するものではありません。
