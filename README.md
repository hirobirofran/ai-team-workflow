# ai-team-workflow

複数の AI を役割分担させて開発を加速するためのワークフローテンプレート集。

コンテキスト爆発・暴走・ケアレスミスを減らし、Claude Code・Gemini・GitHub Copilot を
チームとして機能させる仕組みを提供する。

## 何が入っているか

```text
ai-team/          ← 他リポジトリで使うテンプレート一式
  NOTE.md         ← Claude Code 行動規範（コピーして使う）
  templates/      ← 役割別プロンプトテンプレート
  handoff/        ← 他リポジトリにコピーする handoff 構造の見本
docs/             ← このリポジトリ自体の設計書・ナレッジ
handoff/          ← このリポジトリ自身の作業ファイル（plan/aar/checkpoint）
```

## クイックスタート

```bash
# 使いたいプロジェクトに NOTE.md をコピー
cp ai-team/NOTE.md /path/to/your-project/NOTE.md

# Claude Code 起動後、最初に指示する
# 「NOTE.md を読んでルールを把握して」
```

詳しい使い方は [ai-team/README.md](ai-team/README.md) を参照。

## AI チームのフロー

```text
NotebookLM（リサーチ）→ Web Gemini（PM・設計）→ Claude Code（実装）
                                                      ↓
                                               handoff/issue-XX/
                                               YYYYMMDD_plan.md  ← Copilot 引き継ぎにも使う
                                               YYYYMMDD_aar.md
```

## ドキュメント

- [ビジョン](docs/vision.md) - 解決したい問題と目指す姿
- [プラン](docs/plan.md) - ロードマップと既知の課題
- [ナレッジ](docs/knowledge.md) - 実地での知見・ペインポイント
