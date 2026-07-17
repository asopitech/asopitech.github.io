# Getting started: setup and builds

Follow the [official installation guide](https://nim-lang.org/install.html), then verify the compiler and Nimble:

```bash
nim --version
nimble --version
```

Create `hello.nim`:

```nim
echo "Hello, Nim!"
```

Compile it, or compile and run it:

```bash
nim c hello.nim
nim c -r hello.nim
```

A first release build might look like:

```bash
nim c -d:release -o:bin/myapp src/main.nim
```

For a multi-file project, create Nimble metadata and use its build/test tasks:

```bash
mkdir my-nim-app
cd my-nim-app
nimble init
nimble build
nimble test
```

Use `nim c -r` for a small development loop, run tests in CI, and validate each target platform before adding FFI or alternate backends. Continue with [language basics](language-basics.md).
