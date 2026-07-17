# Type system and memory management

Nim combines inference for local code with explicit types at API boundaries. `object` is generally a value type; use `ref object` for reference semantics.

```nim
type Point = object
  x, y: float64

var a = Point(x: 1, y: 2)
var b = a
b.x = 100 # a.x remains 1
```

Nim 2 uses ORC by default. ORC builds on reference counting and handles cycles; ARC/ORC use destructors and move semantics rather than a classic stop-the-world collector. This differs from Rust: Nim has no borrow checker proving all reference relations statically.
