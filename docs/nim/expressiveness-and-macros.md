# Expressiveness and metaprogramming

Nim combines Python-like indentation with Pascal/Ada-family constructs such as `proc`, `type`, `object`, enums, and range types. UFCS allows both `sort(values)` and `values.sort()`.

Generics, concepts, templates, and AST macros offer multiple abstraction styles. Macros transform syntax trees rather than strings, which is useful for DSLs, serializers, RPC, routing, tests, ORMs, and parser generators. Use them with restraint: excessive macro layers can make diagnostics and debugging harder.
