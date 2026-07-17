# 表現力とメタプログラミング

## Python風だが、Pascal/Ada系でもある

インデントによるブロック、簡潔な`if`、`for`、シーケンス操作はPython経験者にも読みやすい一方、`proc`、`type`、`object`、`enum`、`distinct`、range型はPascal、Ada、Modulaの影響を強く受けています。

```nim
for i in 0..<10:
  echo i

let squared = values.mapIt(it * it)
```

Nimの識別子は、通常、先頭文字以外の大文字小文字とアンダースコアを区別しません。C系・Python系の命名規則をつなぐ際には便利ですが、チームでは命名規約を決める価値があります。

## UFCS・ジェネリクス・concept

自由関数をメソッド風に呼べます。

```nim
sort(values)
values.sort()
text.strip().toLowerAscii()
```

ジェネリクスとconceptは、型パラメータと制約による再利用を可能にします。

```nim
proc sumValues[T: SomeNumber](values: openArray[T]): T =
  for value in values:
    result += value
```

Nimのtype class/conceptはJavaやC#のクラスではなく、ジェネリック制約に近い概念です。オーバーロード、テンプレート、マクロなど複数の抽象化手段があるため、公開APIの設計では一貫性を意識します。

## ASTを扱うマクロ

Nimの差別化要因はコンパイル時メタプログラミングです。マクロは文字列置換ではなくASTを受け取り、変換して返します。

```nim
import macros

macro debug(expr: untyped): untyped =
  result = quote do:
    echo `expr`.astToStr, " = ", `expr`
```

より軽量な繰り返しにはテンプレートを使えます。

```nim
template repeat(count: int, body: untyped) =
  for _ in 0..<count:
    body
```

SQL DSL、シリアライザー、RPC、ルーティング、テスト、ORM、パーサー生成に強力ですが、過用すると展開後の構造、IDE、エラーの発生源を追いにくくします。利用者向けには展開後の振る舞いとデバッグ方法も示しましょう。
