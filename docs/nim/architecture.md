# Language architecture

Nim is best understood as a multi-backend language, not C syntax sugar.

```text
Nim source → parser → AST → semantic analysis / type checking
→ macro expansion / generic instantiation / compile-time evaluation
→ backend → C / C++ / Objective-C / JavaScript
```

C-family outputs are then compiled by target-platform toolchains into native executables or libraries. The JavaScript backend emits JavaScript directly. Macros and generics are resolved before backend generation, so backends do not reimplement Nim's high-level language features.

Keep language-level features—types, control flow, objects, generics, macros, exceptions—separate from backend-specific capabilities such as pointers, C ABI, DOM, Promise, OS APIs, and JS GC. This boundary guides shared-code and FFI design.
