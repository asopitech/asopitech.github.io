# GUI開発

## 結論

Nimには公式標準GUIフレームワークはありません。GUIは外部ライブラリまたはC/C++／Web技術との連携で構築します。Rustのegui、GoのFyne、C#のAvaloniaのような、コミュニティ内で事実上の標準となった基盤はないため、対象OS、UIの性質、配布コストから選ぶ必要があります。

| 用途 | 有力な選択肢 |
| --- | --- |
| Linux中心の一般デスクトップGUI | Owlkettle |
| GTK APIを直接扱う | gintro |
| Windows専用ネイティブGUI | wNim |
| 軽量なHTML/CSSデスクトップGUI | WebView系 |
| ゲーム・ツール・デバッグUI | Dear ImGui |
| 自前描画／デスクトップ・Web共通 | nimx、Fidget |
| Qt/QML資産 | nimqml系 |
| 単純な小型GUI | NiGui（制約を理解して利用） |

## GTK：Owlkettleとgintro

**Owlkettle** はGTK 4とlibadwaitaを使う宣言的UIフレームワークです。状態から`view`でWidgetツリーを組み立てる方式で、Linuxデスクトップ向けの現代的な候補です。

```nim
import owlkettle

viewable App:
  counter: int

method view(app: AppState): Widget =
  result = gui:
    Window:
      title = "Counter"
      Box(orient = OrientY):
        Label:
          text = $app.counter
        Button:
          text = "+"
          proc clicked() = app.counter += 1

brew(gui(App()))
```

GTKの成熟したウィジェットを利用できる反面、GTK 4の導入・配布が必要です。WindowsではMSYS2など、macOSではAppKit本来の外観にならないこと、依存ライブラリ同梱を考慮します。

**gintro** はGTK 3/4とGObject Introspectionの高水準バインディングです。OwlkettleがGTKを宣言的に抽象化するのに対し、gintroはGTK/GNOME APIをより直接細かく扱えます。カスタムウィジェット、GTK周辺ライブラリ、最新GTK機能を使う場合はgintro、アプリを簡潔に構成する場合はOwlkettleが適します。

## Windows：wNim

**wNim** はWin32をNim向けに包むWindows専用GUIフレームワークです。標準コントロール、メニュー、ダイアログ、トレイ、GDI、イベント、自動レイアウトを扱えます。

```nim
import wNim

let app = App()
let frame = Frame(title = "Nim Windows App", size = (500, 300))
let button = Button(Panel(frame), label = "Run")
button.wEvent_Button do (event: wEvent):
  button.label = "Completed"
frame.show()
app.mainLoop()
```

依存が比較的少なく、社内ツール、管理画面、Windows専用ユーティリティに適します。WinUI 3やWindows App SDKを抽象化するものではなく、タッチ、アニメーション、モダンなデザインは追加実装が必要です。

## クロスプラットフォーム描画：NiGui、nimx、Fidget

**NiGui** は基本的なウィンドウ、入力、レイアウト、タイマー、ダイアログ、描画を提供する小型ツールキットです。APIは簡単ですが、機能・イベント・ドキュメントの完成度とmacOS対応に制約があり、新規の本格アプリより小規模な内部ツールや試作向けです。

**nimx** はOS標準ウィジェットを包むのではなく自前描画するフレームワークで、デスクトップ、モバイル、WebGLを視野に入れます。独自デザイン、可視化、ゲームツールに向きますが、アクセシビリティ、IME、標準業務ウィジェット、プロジェクトの継続性を検証する必要があります。

**Fidget** はHTMLとOpenGLを出力先に持つ、Figma風の宣言的レイアウトを目指すUIライブラリです。WebとネイティブのUIロジック共有、ダッシュボード、可視化、独自アプリUIに興味深い一方、GTK/Qt級の汎用GUI基盤ではありません。

## Qt/QMLとWebView

Qtを使うなら、**QML UI + Nimバックエンド** が現実的です。nimqml系はQt Quickのレイアウト・アニメーション・GPU描画をUIに使い、Nimを状態・ネイティブ処理に置きます。Qtの導入・配布・ライセンス、バインディング更新、Qtバージョン差を確認してください。

WebView系ではHTML/CSS/JavaScriptをUIにし、Nimをネイティブバックエンドに置きます。WindowsのWebView2、macOSのWebKit、LinuxのWebKitGTKのようなOS側エンジンを使う軽量な`webview`系は、React/Vue/SvelteなどのWeb資産を生かせます。OSごとのWebView差、Linux依存、JS/Nim間RPC、フロントエンドのビルド、署名・更新・インストーラーは別途設計します。新規開発では更新状況のよいWebViewラッパーを選びます。

## Dear ImGuiと描画基盤

Dear ImGuiはゲームツール、エディタ、モデル調整、監視、デバッグ、可視化に適します。Nimではcimgui経由のバインディングを利用できます。Immediate Mode GUIは描画ループと状態管理を単純にしますが、一般消費者向けフォーム、アクセシビリティ、複雑なテキスト編集、モバイル一般UIには不向きです。

SDL、Raylib、OpenGL、VulkanはGUIフレームワークではなく、ウィンドウ・描画・入力を使って独自UIを構築する基盤です。音楽ツール、グラフ／ノードエディタ、ゲーム、シミュレータ、CAD的UIには向きますが、テーブル、メニュー、IME、アクセシビリティを自作する負担があります。

## モバイルと配布

モバイルGUIの標準基盤はありません。AndroidではKotlin/Java UIとのJNI、WebView、SDL/Raylib/nimx、Qt/QML、iOSではUIKitとのObjective-C連携、WebView、nimx、Qt/QMLを検討します。モバイルを主目的にするなら、SwiftUI、Jetpack Compose、FlutterなどへUIを任せ、Nimをネイティブライブラリとして組み込む方が安定します。

配布も言語だけでは完結しません。WindowsではDLL、アイコン、manifest、署名、MSI/MSIX/Inno Setup/NSIS、WebView2またはGTK DLL群、macOSでは`.app`、`Info.plist`、framework、署名、notarization、Linuxでは依存関係、`.desktop`、AppImage/Flatpak/deb/rpmを用意します。WindowsのGUIアプリは次のようにビルドできます。

```bash
nim c -d:release --app:gui app.nim
```

## 実用的な選び方

- Windows専用の管理ツール: **wNim**。
- Linux中心の一般デスクトップ: **Owlkettle**、細かなGTK操作は**gintro**。
- Windows/macOS/Linuxで現代的なUI: **WebView + HTML/CSS**、Nimをバックエンドに分離。
- 独自UI・可視化: **Dear ImGui**、**nimx**、**Fidget**、**Raylib**。
- Qt資産: **QML + Nim**。

NimのGUIの強みは標準GUIそのものではなく、GTK、Win32、Qt、WebView、SDL、OpenGLへFFIで接続できることです。小型のデスクトップUIや既存GUI基盤のバックエンドとしては実用的ですが、選択肢の分散、保守状況、クロスプラットフォーム配布、アクセシビリティ／IME、モバイル、ビジュアルデザイナーの不足を前提に評価してください。
