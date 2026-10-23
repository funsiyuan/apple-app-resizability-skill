# Apple App Resizability Skill

**Xcode 27.1 に同梱されている App Resizability Agent Skill を、iOS 開発者が参照・利用しやすいように保存したリポジトリです。**

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

> **非公式アーカイブです。** このリポジトリは個人で管理しており、Apple との提携や Apple による承認を意味するものではありません。Skill の内容は Xcode に由来します。再配布・改変の際は、適用される Apple の利用条件をご確認ください。

## これは何？

**App Resizability** は、アプリの実行中に**ウインドウサイズが変化する環境**へ既存の iOS アプリを対応させるための、AI コーディングエージェント向け Skill です。

**iPhone Duo への対応**は代表的な用途の一つですが、それだけに限定されません。Split View などのマルチタスク、Stage Manager、マルチウインドウ、Scene ライフサイクル、レイアウトに関係する API の見直しも対象です。

これは**エージェント向けの指示書と移行ガイド**であり、SDK や実行時ライブラリではありません。Skill を実行するだけで、端末への対応が完了するわけではありません。

## 主な対象

- `UIScreen.main` / `mainScreen` のような従来の画面情報取得を、利用箇所に適した API へ変更する。
- `interfaceOrientation` や `userInterfaceIdiom` に依存したレイアウト判定を見直す。
- Safe Area、特に左右非対称のインセットを適切に扱う。
- Application ライフサイクルへの依存を見直し、Scene ベースの処理へ移行する。
- 画面サイズが固定であることや、ウインドウが一つだけであることを前提にしたコードを検出する。

**Swift と Objective-C** の両方を対象とし、検出パターン、移行手順、検証項目が用意されています。無関係なリファクタリングを避け、必要な変更に範囲を絞る方針です。変更内容のレビューと、シミュレータまたは実機でのサイズ変更テストは別途必要です。

## ファイル構成

```text
.
├── SKILL.md
└── references/
    ├── uiscreen-task.md
    ├── orientation-task.md
    ├── idiom-task.md
    ├── scene-lifecycle-task.md
    └── safe-area-task.md
```

`SKILL.md` に適用条件と実行手順が記載され、`references/` に各タスクの詳細があります。**利用する際はディレクトリ全体を保持してください。** `SKILL.md` だけでは参照先のファイルが不足します。

## 使い方

**方法 1：インストール済みの Xcode からエクスポートする（推奨）**

利用中の Xcode が Skill のエクスポートに対応している場合：

```bash
xcrun agent skills export
```

コマンドの出力と利用するエージェントのドキュメントを確認し、エクスポートした Skill を導入してください。Xcode / SDK のバージョンに合った内容を使いたい場合はこちらがおすすめです。

**方法 2：このリポジトリをエージェントの Skill ディレクトリに配置する**

例として、プロジェクト内の Codex 用 Skill ディレクトリへ配置する場合：

```bash
git clone https://github.com/funsiyuan/apple-app-resizability-skill.git /tmp/apple-app-resizability-skill
mkdir -p .agents/skills/app-resizability
cp -R /tmp/apple-app-resizability-skill/SKILL.md /tmp/apple-app-resizability-skill/references .agents/skills/app-resizability/
```

Skill の検出方法や配置先は、ツールやバージョンによって異なります。各エージェントの最新ドキュメントをご確認ください。`SKILL.md` と参照ファイルを直接エージェントに読ませる方法もあります。

プロンプト例：

> `app-resizability` Skill を使って、この iOS プロジェクトを確認してください。画面サイズ、向き、Safe Area、Scene ライフサイクルに関する古い実装を洗い出し、ウインドウサイズの変更と iPhone Duo への対応に必要な修正を提案してください。手動テストが必要な箇所も示してください。

## Xcode 内の元ファイルを探す

Xcode 内には `.idechatprompttemplate` 形式のテンプレートが含まれる場合があります。保存場所や形式はバージョンによって変更される可能性があります。

```bash
find "/Applications/Xcode.app/Contents" -name 'app-resizability.idechatprompttemplate' -o -name 'app-resizability'
```

別の名前や場所に Xcode をインストールしている場合は、パスを読み替えてください。再利用できる Skill が必要な場合は、対応する Xcode の公式エクスポート機能を優先してください。

## 出典・注意事項

- **出典：** Xcode 27.1 に同梱された App Resizability Skill。このリポジトリはスナップショットであり、Xcode の更新には自動追従しません。
- **Apple 公式資料：** [Preparing your app for iPhone Duo](https://developer.apple.com/documentation/technologyoverviews/preparing-your-app-for-iphone-duo)
- **非公式プロジェクト：** Apple が管理するリポジトリではありません。Apple、Xcode などは Apple Inc. の商標です。
- **著作権と利用条件：** Apple 由来のコンテンツについて、本リポジトリが独自の著作権や再配布許諾を付与するものではありません。原ファイルに適用される条件をご確認ください。

ドキュメントの改善や、Xcode のバージョン間の差異に関する報告を歓迎します。元の Skill を変更する場合は、その差分がわかるようにしてください。
