# Nim: high-level code without leaving the C ground

Nim aims to preserve C-like performance, deployment freedom, and FFI while providing static typing, inference, abstraction, and compile-time metaprogramming. It is not a Rust-style safe-C replacement: ordinary code can be high level, but pointers, C ABI, manual memory management, and inline C remain available when needed.

## Guide structure

This is a **Nim Architecture Guide**, organized around the site map rather than a duplicate of the official reference:

1. **Introduction** — [overview](index.md), [guide scope](guide-scope.md), and [design philosophy](design-and-safety.md).
2. **Tutorial** — [setup and builds](getting-started.md) and [language basics](language-basics.md).
3. **Language architecture** — [compiler and backend model](architecture.md).
4. **Backends and FFI** — [C interoperability](compilation-and-ffi.md), [JavaScript](javascript-backend.md), and [WASM/MoonBit](wasm-and-moonbit.md).
5. **Types, memory, and compile-time programming** — [type/ORC/ARC model](types-and-memory.md) and [macros](expressiveness-and-macros.md).
6. **Runtime, web, GUI, and AI** — [async/NimScript](runtime-and-tooling.md), [Nim JS engines](javascript-engines.md), [GUI development](gui.md), and [GPU/LLMs](gpu-and-llm.md).
7. **Comparisons and practice** — [C/Rust/Zig](comparisons.md) and [adoption](adoption.md).

Tutorials, standard library, GUI, systems, ecosystem, source reading, roadmap, and appendix material are planned in the [guide scope](guide-scope.md).

Nim's stated values are **Efficient, Expressive, Elegant**. Consult the [official site](https://nim-lang.org/), [manual](https://nim-lang.org/docs/manual.html), [tutorial](https://nim-lang.org/docs/tut1.html), and [standard library](https://nim-lang.org/docs/lib.html) for authoritative details.
