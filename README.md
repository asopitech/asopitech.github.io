# asopitech.github.io

[asopitech](https://github.com/asopitech) のプロフィールサイトと、**Nim Architecture Guide**（日英対訳）のソースです。/
The profile site of [asopitech](https://github.com/asopitech), and the source of the bilingual **Nim Architecture Guide**.

<p align="center">
  <a href="https://asopitech.github.io/">🌐 English</a> &bull;
  <a href="https://asopitech.github.io/ja/">🌐 日本語</a> &bull;
  <a href="https://asopitech.github.io/nim/">📘 Nim Architecture Guide</a> &bull;
  <a href="https://github.com/sponsors/asopitech">❤️ Sponsor</a>
</p>

<p align="center">
  <a href="https://github.com/asopitech/asopitech.github.io/actions/workflows/deploy.yml"><img alt="Deploy" src="https://img.shields.io/github/actions/workflow/status/asopitech/asopitech.github.io/deploy.yml?branch=main&label=pages%20deploy"></a>
  <img alt="MkDocs Material" src="https://img.shields.io/badge/MkDocs-Material-526CFE?logo=materialformkdocs&logoColor=white">
  <img alt="i18n" src="https://img.shields.io/badge/i18n-en%20%2F%20ja-E57FC1">
</p>

## 中身 / What is here

| | |
|:--|:--|
| **Nim Architecture Guide** | Nim の言語アーキテクチャ、バックエンドと FFI、型システムとメモリ管理、マクロ、ランタイムとツール、GUI、GPU/LLM、C・Rust・Zig との比較、採用判断まで。全ページ **英語と日本語の対訳**です / Nim's language architecture, backends and FFI, type system and memory, macros, runtime and tooling, GUI, GPU and LLMs, comparisons with C, Rust and Zig, and adoption trade-offs — every page in both English and Japanese |
| **プロフィール / Profile** | 取り組んでいるプロジェクトと組織の一覧 / what is being built, and where |

## ローカルで動かす / Run it locally

```bash
pip install -r requirements.txt
mkdocs serve
```

<http://127.0.0.1:8000> で開きます。英語が `/`、日本語が `/ja/` です。/
English is served at `/`, Japanese at `/ja/`.

```bash
mkdocs build   # 本番と同じビルド / the same build CI runs
```

## 構成 / Layout

| Path | 内容 / Contents |
|:--|:--|
| `docs/index.md` / `docs/index.ja.md` | トップページ（英 / 日）/ the landing page in each language |
| `docs/nim/*.md` / `docs/nim/*.ja.md` | Nim Architecture Guide の各章 / the guide's chapters |
| `docs/assets/brand/` | ロゴ・ファビコン・ソーシャルカード / logo, favicon, social card |
| `overrides/main.html` | OGP・Twitter カード・同意バナーの差し込み / share metadata and the consent banner |
| `mkdocs.yml` | ナビゲーション、`mkdocs-static-i18n` の言語設定、日本語ナビ訳 |
| `.github/workflows/deploy.yml` | `main` への push で GitHub Pages へデプロイ / deploys to Pages on push to `main` |

## 翻訳の約束 / The translation rule

ページは `name.md`（英語）と `name.ja.md`（日本語）の対で管理しています
（`mkdocs-static-i18n` の suffix 方式）。**片方だけ更新した状態は未完成**です。
新しいページを追加したら `mkdocs.yml` の `nav` と `nav_translations` の両方に登録してください。/
Pages are kept as `name.md` (English) and `name.ja.md` (Japanese) pairs. Updating one
without the other leaves the page half-done, and a new page needs an entry in both
`nav` and `nav_translations`.

## ブランド / Brand

ロゴ・ファビコン・ソーシャルカードは
[`asopitech-labs/.github`](https://github.com/asopitech-labs/.github/tree/main/assets/brand)
で生成したものをコピーしています。直接編集せず、向こうを更新してください（`BRAND.md` 参照）。/
The brand assets are generated in
[`asopitech-labs/.github`](https://github.com/asopitech-labs/.github/tree/main/assets/brand)
and copied here — update them there, not here.

## Support

プロジェクトが役に立ったら、開発を応援していただけると嬉しいです 🚀 /
If this has been useful, support for continued development is welcome.

<p align="center">
  <a href="https://github.com/sponsors/asopitech?metadata_source=asopitech_io_readme&metadata_campaign=sponsor_early_access&metadata_content=readme_sponsor_section"><img alt="Sponsor @asopitech on GitHub Sponsors" src="https://img.shields.io/badge/GitHub%20Sponsors-%E2%9D%A4%20Sponsor%20%40asopitech-db61a2?logo=githubsponsors&logoColor=white&style=for-the-badge"></a>
  <a href="https://buymeacoffee.com/asopitechia"><img alt="Buy Me A Coffee" src="https://img.shields.io/badge/Buy%20Me%20a%20Coffee-%E2%98%95-FFDD00?style=for-the-badge&logo=buymeacoffee&logoColor=black"></a>
</p>
