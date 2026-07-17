# Errors, concurrency, and NimScript

Exceptions are a standard error-handling option, and `raises` can restrict declared exceptions. Nim supports threads, async I/O, channels, and thread pools. Because ARC/ORC centers non-atomic reference counting, design cross-thread sharing deliberately.

NimScript supports build and automation tasks:

```nim
task build, "Build":
  exec "nim c -d:release src/app.nim"
```

This lets applications, libraries, compile-time work, and build scripting share a familiar language style.
