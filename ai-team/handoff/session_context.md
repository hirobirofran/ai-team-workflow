# セッションコンテキスト引き継ぎ

作成日: 2026-04-14

## このリポジトリについて

複数の AI を役割分担させる「AI チームワークフロー」の運用テンプレート集。
コンテキスト爆発・暴走・ケアレスミスを減らすことが目的。

## 背景・経緯

- ユーザー: hirobirofran さん（Terraform/Terragrunt の開発者）
- 課題: Web Gemini と GitHub Copilot Agent が長い作業で混乱・暴走する
- 解決策: AI をフェーズ別に分担させ、構造化された成果物（md ファイル）を橋渡しにする

## 設計の核心

**「コンテキストではなく成果物を渡す」**

各 AI には全会話ではなく、担当フェーズの入力ファイルだけを渡す。
これにより各 AI のコンテキストを最小に保つ。

## 各ファイルの役割

| ファイル | 役割 |
| --- | --- |
| `CLAUDE.md` | Claude Code の行動規範（整理優先・推測禁止・ハンドオフ管理） |
| `templates/gemini_pm.md` | Web Gemini を PM 役にするプロンプト（Gem 化推奨） |
| `templates/gemini_design.md` | Web Gemini を設計役にするプロンプト（Gem 化推奨） |
| `templates/claude_code_session.md` | Claude Code セッション開始テンプレート |
| `templates/aar.md` | 作業終了後の AAR（After Action Review）フォーマット |
| `handoff/` | AI 間の成果物置き場（.gitignore 済み） |

## CLAUDE.md の使い方

```bash
# 個人用（推奨）: 全プロジェクト共通設定として配置
cp ai-team/CLAUDE.md ~/.claude/CLAUDE.md

# チーム展開後: プロジェクトルートに配置
cp ai-team/CLAUDE.md ./CLAUDE.md
```

## AI チームフロー

```text
Web Gemini (PM役) → spec.md → Web Gemini (設計役) → task_spec.md → Claude Code (実装) → aar.md
```

各フェーズで **新規チャットを開く**（同じチャットを引き継がない）。

## 新セッションでの復元手順

1. このファイル（session_context.md）を Claude Code に読ませる
2. `README.md` を読ませる
3. 「これらのテンプレートについて質問があれば聞いて」で会話再開

## ユーザーの今後の予定

- **会社（Claude Code ライセンス待ち）**: Terraform/Terragrunt 開発。ライセンス取得後は Claude Code をメインに。
- **個人（このリポジトリ）**: AI チームワークフローの整備。将来的に Garmin データ分析などの個人データ統合プラットフォームを構築予定。

## 重要な設計判断（経緯）

- Gemini CLI は「文脈圧縮ツール」として使う（500 行超のファイルを要約させてから Claude Code に渡す）
- GitHub Copilot には要件理解を期待しない。Claude Code が生成した task_spec.md を見せて補完に使う
- handoff/ は .gitignore する。AAR の履歴を残したい場合は個人ブランチで管理
- フォルダ名は個人試用中は `ai-team-yourname/`、チーム展開時に `ai-team/` にリネーム
