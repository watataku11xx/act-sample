# CI/CD (GitHub Actions + act)

このリポジトリは GitHub Actions を使った CI を同梱し、`act` でローカル実行できるよう初期セットアップ済みです。

## 前提
- Docker がインストール・起動済み
- `act` がインストール済み（macOS 例）
  - Homebrew: `brew install act`
  - バイナリ: https://github.com/nektos/act/releases

## 使い方
- ワークフロー: `.github/workflows/ci.yml`
- act 用イメージマッピング: `.actrc`（`ubuntu-latest` などをローカルで解決）
- シークレット: `.secrets.example` を `.secrets` にコピーして値を設定

### よく使うコマンド
- 一覧表示: `act -l`
- ドライラン: `act -n`
- `push` イベント相当を実行: `act push`
- 特定ジョブのみ実行: `act -j build-test`
- シークレットを読み込んで実行: `act --secret-file .secrets push`

## ワークフローの内容（概要）
- Node.js プロジェクト（`package.json` が存在）: `npm ci` / `npm test`
- Python プロジェクト（`requirements.txt` or `pyproject.toml`）: `pip`/`poetry` で依存解決、`pytest`/`ruff`
- Go プロジェクト（`go.mod`）: `go test ./...`
- Rust プロジェクト（`Cargo.toml`）: `cargo test`
- 上記に該当しない場合はプレースホルダーのメッセージのみ

## カスタマイズ
- 言語やツールが増えたら `.github/workflows/ci.yml` にジョブ/ステップを追加してください。
- ランナーイメージは `.actrc` の `-P ubuntu-****=ghcr.io/catthehacker/ubuntu:act-****` で調整できます。

