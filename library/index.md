# library/

> **状態**: 暫定（[#19](https://github.com/hirobirofran/ai-team-workflow/issues/19) の最初の試行、[#28](https://github.com/hirobirofran/ai-team-workflow/issues/28) の PoC として実地検証中）
> **位置づけ**: このリポジトリ固有の情報置き場。`ai-team/` はテンプレ専用なので、本フォルダはここ（ルート）に置く。

## 目的

Web 検索や AI 学習データだけでは追いきれない「生物」情報のスナップショット集約場所。
ライブラリのアーカイブ状態、CLI の破壊的変更、モデルの新旧、MCP の認証方式など、
**古い情報に埋もれやすい一次情報** を定期的に拾って残す。

## 運用ルール（暫定）

### pull 型運用

- 困ったときに参照される側に回る
- push で網羅しようとしない（認知制約で続かない）
- 対象は AI ツール・モデル・プロトコルなど **本リポジトリのテンプレが依存するもの**

### ログ→原則の 2 段階蓄積

- 生のリサーチ記録: `handoff/issue-XX/YYYYMMDD_research.md`
- 熟成・整理後の恒久情報: 本フォルダのカテゴリ md
- 熟成タイミングは [#21](https://github.com/hirobirofran/ai-team-workflow/issues/21)（自己ナレッジループ）の設計と連動（要検討）

### 変更は PR 経由

- 軽量メモは `handoff/followup.md`（直接 push 可）
- 本フォルダは参照される情報なので、ブランチ + PR の通常フロー

## カテゴリ（暫定）

| ファイル | 範囲 |
|----------|------|
| [ai-tooling-ecosystem.md](ai-tooling-ecosystem.md) | Gemini CLI / Codex / Copilot など AI コーディング支援ツール群 |

カテゴリは必要になったら追加（暫定なので破壊的変更可）。

## 記載フォーマット（推奨）

各エントリは時系列降順（最新が上）で:

```markdown
### YYYY-MM-DD: トピック名

- **情報源**:
  - [一次情報 URL]
- **要点**: 1〜3 行
- **このリポジトリへの影響**: テンプレ更新が必要な箇所
```

## 関連 Issue

- [#19](https://github.com/hirobirofran/ai-team-workflow/issues/19) library/ 新設（本フォルダの起点）
- [#21](https://github.com/hirobirofran/ai-team-workflow/issues/21) 自己ナレッジループ（熟成設計と直結）
- [#28](https://github.com/hirobirofran/ai-team-workflow/issues/28) Gemini CLI で DeepResearch ができるか調査（library/ 運用 PoC）
