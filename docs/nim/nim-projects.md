# Two Nim Projects in Progress: Nimino and Nimculus

Nim is attractive because it combines Python-like readability with the placement freedom and low-level reach of C. To validate those qualities in real application development, I have started two projects in Nim.

- [Nimino](https://github.com/asopitech-labs/nimino) — a lightweight cross-platform WebView desktop application foundation hosted by Nim.
- [Nimculus](https://github.com/asopitech-labs/nimculus) — a fast GPU-native code editor built with Nim and NimNUI.

Both projects explore how far Nim can go as a practical GUI and desktop application platform. They are not just samples; they are experimental grounds for evaluating Nim across OS APIs, WebView integration, GPU rendering, packaging, and developer experience.

The implementation direction is also informed by Rust ecosystem precedents. Nimino uses [Tauri](https://tauri.app/) as an important reference for lightweight WebView-based desktop applications, while Nimculus uses [Zed](https://zed.dev/) as a reference point for GPU-accelerated editor architecture and developer experience. The goal is not to clone either project, but to learn from their product and architecture choices and reinterpret those ideas through Nim.

## Why Nim?

Nim occupies an interesting position for desktop application foundations.

- It **compiles to native code**, which can make deliverables smaller than VM-first distributions.
- It has strong **C ABI interoperability**, making Win32, GTK, WebKitGTK, WebView2, and other OS or GUI APIs approachable.
- Its **macros and templates** make it possible to place thin, safer abstractions on top of low-level FFI.
- Its **Python-like syntax** helps experiments move quickly.

At the same time, Nim's GUI and desktop space often feels less like choosing one huge ready-made framework and more like selecting and connecting the layers you need. That is why I started from two directions: a WebView application foundation and a GPU-native editor.

## Nimino: a WebView Desktop Foundation with Thin FFI

Nimino is a framework for handling native windows and WebViews from Nim through a thin FFI layer close to the underlying OS APIs.

The goal is not to bundle an entire Chromium runtime like Electron. Instead, Nimino aims to use platform WebView infrastructure such as WebView2 Evergreen Runtime on Windows and WebKitGTK on Linux. This keeps the application lightweight while preserving native integration and giving Nim-side code direct control over the host. In that sense, Tauri is a key reference for the product shape: a small native shell, platform WebViews, explicit permissions, and a clear boundary between the web UI and the native host.

### Planned Components

Nimino is being developed with the following responsibilities in mind.

| Component | Role |
| --- | --- |
| `nimino-native` | Thin window and WebView layer using Win32/WebView2 and GTK/WebKitGTK directly |
| `nimino-core` | Application lifecycle, RPC, profiles, navigation policy, and permission policy |
| `nimino-wsl` | Adapter connecting Nim processes running in WSL to a Windows GUI host |
| `nimino-pack` | CLI for generating applications from a URL or manifest |

The initial targets are Windows, native Linux, and WSL. macOS is considered a future extension point.

### What Nimino Is Testing

Nimino is intended to test several ideas.

- Treat WebView as an OS-provided UI component rather than a large application runtime.
- Keep lifecycle, RPC, permissions, profiles, and packaging responsibilities on the Nim side.
- Provide a safe way for Nim processes in WSL to operate a Windows-side GUI host.
- Combine the speed of Web UI development with the lightness of native desktop distribution.

If Nimino stabilizes, it can become a useful foundation for internal tools, admin consoles, local AI utilities, and developer tools where a Web UI is enough, but a browser tab is not the desired delivery format.

## Nimculus: Toward a GPU-Native Code Editor

Nimculus is a GPU-native code editor built with Nim and NimNUI.

A code editor is a comprehensive application: text processing, rendering, input, file watching, LSP integration, extension boundaries, settings, themes, and performance all matter. Building an editor in Nim is therefore a broad practical test of the language, runtime, and GUI stack. Zed is the main reference for this direction, especially its emphasis on GPU rendering, responsiveness, collaboration-minded design, and a modern developer workflow.

### Nimculus Design Direction

Nimculus focuses on the following direction.

- A fast rendering pipeline designed around GPU usage.
- Lightweight native executables that fit Nim's strengths.
- Clear boundaries between the editor core, UI, and plugin surface.
- A development experience built around LSP and external tool integration.
- Validation of Nim-native GUI application development through NimNUI.

The project is still early, but Nimculus is positioned as a benchmark project for building serious developer-facing desktop applications in Nim.

## How the Two Projects Relate

Nimino and Nimculus point in different directions.

- Nimino uses WebView to build a lightweight desktop application foundation.
- Nimculus uses GPU-native UI to build a high-performance code editor.

However, both projects share the same question: can Nim deliver a practical desktop experience? Lessons from Nimino around OS integration, packaging, RPC, and lifecycle management can inform Nimculus. Lessons from Nimculus around input handling, rendering, and editor-core design can deepen the understanding of Nim's GUI ecosystem as a whole.

## Roadmap

The immediate plan is to build small working pieces and iterate.

1. Stabilize the basic Window, WebView, RPC, and packaging path in Nimino.
2. Build the minimal rendering, text buffer, input, and file operation loop in Nimculus.
3. Test the projects on Windows, Linux, and WSL while evaluating Nim's FFI, async model, memory management, and build experience.
4. Feed the findings back into this Nim Architecture Guide.

Nim is less about relying on a single huge ecosystem and more about connecting low-level and high-level layers yourself. Nimino and Nimculus are projects for testing that connective power in product-like forms.

## Repositories

- [asopitech-labs/nimino](https://github.com/asopitech-labs/nimino)
- [asopitech-labs/nimculus](https://github.com/asopitech-labs/nimculus)
