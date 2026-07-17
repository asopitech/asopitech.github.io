# JavaScript backend

Nim's JavaScript backend generates JavaScript source directly after Nim type checking, macro expansion, and generic instantiation. It is not a WASM backend.

```text
Nim source → Nim compiler → JavaScript → browser / Node.js / Electron
```

Compile with:

```bash
nim js app.nim
nim js -r app.nim
nim js -d:release -o:public/app.js src/app.nim
```

It usually emits a consolidated JavaScript output rather than a one-to-one ESM module graph. Use a JavaScript/TypeScript wrapper and a bundler such as esbuild, Rollup, or Vite when packaging public APIs or integrating npm dependencies.

## Strengths

Nim's static types, inference, enums, `distinct`, variant objects, generics, concepts, templates, and macros remain available before code generation. Use `importjs`, `jsffi`, `dom`, `asyncjs`, and `jsfetch` to work with JavaScript APIs, DOM, Promise-based async code, and Fetch. This is a good target for parsers, validators, DSLs, state machines, text transforms, browser logic, and Node.js tools.

## Boundaries

The JavaScript backend uses the host JS runtime and GC. It does not provide native pointer operations, manual memory management, OS APIs, native threads, or direct C-library linking. Be deliberate about 64-bit values, overflow-sensitive code, Unicode/string indexing, external-resource lifetime, Promise rejection, and npm/ESM integration.

Choose Nim JS when DOM, Fetch, Promise, JavaScript objects, and string-heavy logic are central. Compare WASM when C/C++ libraries, memory layout, binary/SIMD work, or native-style numerical processing matters.

## Why it exists

Nim is a multi-backend language centered on its typed internal representation, not a C-only language. JavaScript enables shared client/server language features while preserving the option to compile server-side code through C to native binaries. Its aim is to carry Nim's type system and metaprogramming into JS environments, not to replace the entire TypeScript frontend ecosystem.
