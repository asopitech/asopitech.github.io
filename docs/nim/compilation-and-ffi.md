# Compilation and C interoperability

Nim commonly compiles through C to a native executable:

```text
Nim source → Nim compiler → C source → C compiler → native executable
```

```bash
nim c app.nim
nim c -r app.nim
nim c -d:release app.nim
nim cpp app.nim
nim js app.nim
```

C is a backend, not a thin syntax target: type checking, generic instantiation, macro expansion, and memory-management insertion happen first. Use `importc` to import C declarations and `exportc` to expose a C ABI. Keep ABI boundaries to fixed-width integers, pointers plus lengths, C-compatible structs, opaque handles, explicit lifecycle functions, and error codes.
