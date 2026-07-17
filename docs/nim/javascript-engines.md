# JavaScript engines written in Nim

**Bali** is an experimental JavaScript engine implemented in Nim, with lexer, parser, ECMAScript execution work, and Test262 tracking. **Sev** adds host/runtime concerns on top of Bali; **Ferus** is an experimental Nim web-engine project that uses Bali. **Mirage** is related IR/bytecode work.

These projects are valuable for language-runtime, sandbox, DSL, and browser-engine research, but they are not production alternatives to QuickJS, Node.js, or V8. For embedding JavaScript in a production Nim application, prefer an established engine exposed through C FFI, such as QuickJS or Duktape.

Rust's **Boa** is more mature as an embeddable engine: it has bytecode/register-VM architecture, a dedicated GC, modular crates, and stronger ECMAScript-conformance work. Bali and Boa have different primary goals: Bali explores a Nim-native browser/JS stack, while Boa prioritizes an embeddable Rust engine.

## Boa, Nova, and Deno

Do not group these as equivalent Rust JavaScript engines. **Boa** is an embeddable Rust engine with parser, AST, bytecode compiler, register VM, dedicated GC, and runtime crates. **Nova** is an in-progress Rust JavaScript/WASM engine. **Deno** has a Rust runtime, but its JavaScript engine is the C++ V8 engine.

Boa is ahead of Bali in embedding APIs, bytecode-VM maturity, and ECMAScript-conformance work. Neither Boa nor Bali should be treated as Node.js or npm-compatible by default. Choose Bali/Sev for Nim-native runtime, sandbox, DSL, and web-engine research; choose an established C-API engine such as QuickJS/Duktape for direct production embedding from Nim, or Boa when the embedding boundary naturally lives in Rust.
