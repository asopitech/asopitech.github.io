# はじめる：セットアップとビルド

## インストール

OSごとの導入方法は、まず[公式インストールガイド](https://nim-lang.org/install.html)を確認してください。導入後、コンパイラとNimbleが利用できることを検証します。

```bash
nim --version
nimble --version
```

NimbleはNimに付属するパッケージマネージャーです。チーム開発では、利用するNimのバージョンと導入手順をREADMEやCIで固定し、ローカルとCIの差を減らします。

## Hello, world

`hello.nim`を作成します。

```nim
echo "Hello, Nim!"
```

ネイティブ実行ファイルをコンパイルします。

```bash
nim c hello.nim
```

`hello`（Windowsでは通常`hello.exe`）が生成されます。コンパイル成功後に実行まで行うには`-r`を付けます。

```bash
nim c -r hello.nim
```

リリース向けの最初のビルドは次の形です。

```bash
nim c -d:release -o:bin/myapp src/main.nim
```

`-d:release`はリリース向けのコンパイル定義を有効にします。最適化、デバッグ情報、実行時チェックの扱いはプロジェクトとコンパイラ設定で確認してください。速度を理由にチェックを無効化する前に、テストとプロファイルで必要性を判断します。

## 最小プロジェクト

複数ファイルのアプリケーションやライブラリでは、Nimbleでパッケージ定義を管理します。

```bash
mkdir my-nim-app
cd my-nim-app
nimble init
nimble build
nimble test
```

`nimble init`は対話的に`.nimble`ファイルを作成します。生成されたパッケージ名、ソース配置、タスクを確認し、プロジェクト規約に合わせて編集してください。依存関係の追加とタスクの詳細は[Nimble](https://github.com/nim-lang/nimble)のドキュメントを参照します。

## 開発ループ

1. `nim c -r src/main.nim`で小さくコンパイル・実行する。
2. テストを追加し、Nimbleプロジェクトでは`nimble test`をCIでも実行する。
3. 本番向けは`-d:release`で対象OSごとにビルドする。
4. Cライブラリや別バックエンドを導入する前に、対象環境で最小の縦切りを通す。

次は[基本構文とステートメント](language-basics.md)で、値、型、プロシージャ、分岐、繰り返し、モジュールを確認します。
