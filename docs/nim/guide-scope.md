# Nim Architecture Guide: scope

This site is an **Architecture Guide**, not a replacement for the official Nim reference. It organizes the design rationale and practical trade-offs that tend to be scattered across manuals, blog posts, forums, and issue trackers.

It is organized around five themes: **Why Nim?**, **Inside Nim**, **Nim vs X**, **Nim in Practice**, and **Nim Ecosystem Watch**. The core model is:

```text
Nim source → parser / AST / semantic analysis
→ macro expansion / generic instantiation
→ backend (C / C++ / Objective-C / JavaScript)
```

Future sections will cover tutorials, types, memory, compile-time programming, FFI, standard library, GUI, web, systems, ecosystem, practice, source reading, roadmap, FAQ, and glossary. Use official documentation for exhaustive language semantics and version-specific behavior.
