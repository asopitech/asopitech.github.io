# Language basics and statements

Nim uses indentation for blocks. Prefer `let` for immutable bindings, `var` for mutable state, and `const` for compile-time constants.

```nim
let name = "Nim"
var count = 0
const bufferSize = 4096
count += 1
```

Define procedures with `proc`:

```nim
proc greet(name: string): string =
  "Hello, " & name & "!"
```

Use `if`/`elif`/`else`, `for`, and `while` as expected. `1..5` includes the end and `0..<5` excludes it.

```nim
for number in 1..5:
  if number mod 2 == 0:
    echo number
```

Model data with `enum`, `object`, and sequences:

```nim
type
  Status = enum pending, running, completed
  Article = object
    title: string
    tags: seq[string]

let article = Article(title: "Nim", tags: @["language", "native"])
```

Export a module symbol with `*` and import it elsewhere:

```nim
# mathutil.nim
proc double*(value: int): int = value * 2
```

See the [type and memory guide](types-and-memory.md) and [official manual](https://nim-lang.org/docs/manual.html) for the detailed model.
