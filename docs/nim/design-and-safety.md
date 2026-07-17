# Design and safety

## Safer than C, not Rust-level safety

`distinct`, range types, enums, fixed-size arrays, and variant objects express constraints that plain C types cannot.

```nim
type UserId = distinct int
type Percentage = range[0..100]
```

Nim does not enforce a safe subset. `nil`, raw pointers, `cast`, manual allocation, external C, and disabled runtime checks remain available. It therefore provides a high-level default path with low-level escape hatches, rather than Rust's borrow-checked safety model.
