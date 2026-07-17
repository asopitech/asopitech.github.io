# GUI development

Nim has no official standard GUI framework. Choose an external toolkit according to target OS, UI style, and packaging constraints.

| Need | Strong candidates |
| --- | --- |
| Linux-first desktop app | Owlkettle; gintro for direct GTK access |
| Windows-native utility | wNim |
| HTML/CSS desktop UI | a maintained WebView wrapper |
| Tools, editors, debug UI | Dear ImGui |
| Custom-drawn desktop/web UI | nimx or Fidget |
| Qt assets | QML with Nim backend |
| Small prototype | NiGui, with its limitations understood |

Owlkettle provides declarative GTK 4/libadwaita UI; gintro exposes GTK/GObject APIs more directly. wNim is a Win32-oriented framework well suited to Windows utilities. nimx and Fidget favor custom rendering over native widgets. QML plus Nim is usually more practical than trying to wrap all Qt C++ APIs. WebView lets Nim provide native logic while web technologies render the UI.

Dear ImGui and SDL/Raylib/OpenGL/Vulkan suit tooling, visualization, and custom graphics, not conventional accessible form applications. Mobile has no standard Nim UI stack; use platform UI frameworks or Flutter-like solutions for UI and embed Nim as a native library where appropriate.

Packaging remains platform work: DLLs/signing/installers on Windows, `.app` bundles/signing/notarization on macOS, and dependency/package formats on Linux. For a Windows GUI binary:

```bash
nim c -d:release --app:gui app.nim
```

Nim's strength is its ability to connect to GTK, Win32, Qt, WebView, and graphics stacks through FFI—not a single standard GUI solution. Validate maintenance, accessibility, IME, deployment, and target-platform behavior before committing to a toolkit.
