# WebAssembly and MoonBit

## Bottom line

MoonBit is the clearer choice when WebAssembly is the primary goal. Nim offers broader native and C/C++ integration: its usual WASM route is Nim → C/C++ → Emscripten → WASM, while MoonBit directly targets `wasm` and `wasm-gc`.

| Concern | Nim | MoonBit |
| --- | --- | --- |
| WASM role | Distribution target through C/C++ tooling | First-class target |
| C/C++ integration | Very strong | Native/C support; WASM is host-FFI oriented |
| Native use | Strong | Supported, but not the primary focus |
| AST macros | Very strong | Not as macro-centric |

Nim's generated C, runtime, memory management, Emscripten runtime, and JS glue can affect size and startup. MoonBit's `wasm-gc` can use GC reference types, but FFI boundaries still require deliberate design.

For a SQL parser, retain Nim when native/Rust C ABI integration, macros, C/C++ libraries, and code reuse matter. Evaluate MoonBit when browser WASM, small binaries, startup time, JS string exchange, and WASM GC dominate. Measure the existing Nim WASM version before porting: size, startup, parse throughput, transfer time, peak memory, AST-return cost, and diagnostics.
