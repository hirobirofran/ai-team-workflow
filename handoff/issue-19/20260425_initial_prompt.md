# Issue #19 新規セッション起動プロンプト

> **作成**: 2026-04-25  
> **用途**: Issue #19（library/ 新設）の作業を新規 Claude Code セッションで開始するときに、ユーザーがそのままコピペして使う初動プロンプト  
> **位置づけ**: このセッション（前セッション）と新セッションを橋渡しする handoff 用のファイル  
> **iteration**: ユーザーと前セッション Claude Code でこのファイルを編集してから新セッションへ渡す

---

## 新セッションへの注記

**このファイル全体（下の「---」以下）をコピーして新セッションの最初の発言に貼り付けてください。**

新セッション起動後、Claude Code は **必ず最初に「質問の有無」を確認してから着手する**こと。
ユーザーが質問に答えてからプラン提示・実装に進むフロー。

---

ai-team-workflow リポジトリで、このプロジェクト固有の `library/` をルート直下に新設する作業を行います。
（Issue #19 が起点だが、#19 本文は `ai-team/library/` と書いてしまっており、それは誤り。
`ai-team/` はテンプレ専用なので汚さない。本作業の中で Issue #19 の本文も訂正する）。

このリポジトリ自体の運用ルールは CLAUDE.md を参照（既に自動ロード済み）。
リサーチ手順は @ai-team/templates/researcher.md を参照してください。

# このリポジトリのフォルダ規約（再確認）

| 場所 | 役割 |
|------|------|
| `ai-team/` | 他リポジトリ向けテンプレ（クリーンに保つ・触らない） |
| `docs/` | このリポジトリの設計書 |
| `handoff/` | このリポジトリの作業ファイル |
| `library/`（今回新設） | このリポジトリ固有の「生物」情報スナップショット |

# タスク

## メインリサーチ：「Gemini CLI で DeepResearch ができるか？」

調査対象:

- Gemini CLI（github.com/google-gemini/gemini-cli）から DeepResearch を呼び出せるか
- 呼べないなら代替手段（API grounding / 別ツール経由など）
- 一次情報（GitHub README・Issues・Releases / Google 公式 docs）で裏取り

調査手順:

1. researcher.md のフォーマットに沿って進める
2. 不明な前提が3つ以内であれば、まず私（人間）に質問
3. 第一段：Web 検索ベース（WebFetch 等）で概要と一次情報リンクを取得
4. 必要に応じて gh コマンドで `google-gemini/gemini-cli` を直接確認
5. 浅ければ「Gemini DeepResearch 用プロンプト案」を生成（実行は人間が行う）

## 成果物（保存先）

- プラン: `handoff/issue-19/YYYYMMDD_plan.md`
- リサーチログ: `handoff/issue-19/YYYYMMDD_research.md`（researcher.md フォーマット）
- 恒久情報: `library/ai-tooling-ecosystem.md`（新規作成、「Gemini CLI と DeepResearch」の節を含める）
- library/ 骨格: `library/index.md`（下記の暫定内容で新規作成）
- AAR: `handoff/issue-19/YYYYMMDD_aar.md`

## library/index.md の暫定骨格（このまま新規作成して）

````markdown
# library/

> **状態**: 暫定（[#19](https://github.com/hirobirofran/ai-team-workflow/issues/19) の最初の試行）
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
### YYYY-MM-DD: [トピック名]

- **情報源**: [一次情報 URL]
- **要点**: 1〜3 行
- **このリポジトリへの影響**: テンプレ更新が必要な箇所
```

## 関連 Issue

- [#19](https://github.com/hirobirofran/ai-team-workflow/issues/19) library/ 新設（本フォルダの起点）
- [#21](https://github.com/hirobirofran/ai-team-workflow/issues/21) 自己ナレッジループ（熟成設計と直結）
````

## 副次タスク：Issue #19 と関連箇所の path 訂正

- Issue #19 本文の `ai-team/library/` 表記を `library/` に訂正（`gh issue edit`）
- `README.md` / `CLAUDE.md` の構造図に `library/` を追記（このプロジェクト固有として）
- `handoff/followup.md` の関連エントリに「`ai-team/` はテンプレ汚染回避のため、`library/` はルート」と注記

## 重要な制約

- リサーチは Claude Code 側で完結させる方針（人間に Web ブラウザ操作を求めない）
- 一次情報 URL は最終確認のため一覧化して報告
- Opus 全振り想定。**同一セッション内でモデル切替しない**（参照: `docs/knowledge.md` の 2026-04-25 ログ）
- トークン圧迫の兆候があれば `handoff/issue-19/YYYYMMDD_checkpoint.md` にスナップショット保存して新セッションへ
- ブランチを切って作業（`feature/issue-19-library-bootstrap` 等）、最後に PR 作成（マージは私が判断）

## 終了条件

- [ ] `library/index.md` が暫定骨格で作成済み
- [ ] `library/ai-tooling-ecosystem.md` に「Gemini CLI と DeepResearch」の節が一次情報リンク付きで埋まっている
- [ ] `handoff/issue-19/` 以下に plan.md / research.md / aar.md が揃っている
- [ ] Issue #19 本文の path 表記が訂正されている
- [ ] `README.md` / `CLAUDE.md` に `library/` が反映されている
- [ ] PR が作成されている

# 開始手順

**まず以下を読んで状況把握**してください:

1. `CLAUDE.md`（既にロード済み）
2. `ai-team/templates/researcher.md`
3. `docs/knowledge.md` の以下の節
   - 「リサーチは『オプション』ではなく最重要フェーズ」
   - 「テンプレが実際に効いた証拠（実地観察 2026-04-25）」
   - 「ログ: 2026-04-25 反例（規約該当判定タスクで Sonnet 破綻）」
   - 「ログ: 2026-04-25 セッション分離の一般化」
   - 「ログ: 2026-04-25 今日の失敗の完全な経緯」
4. `handoff/followup.md`（library/ の動機補強・AI-hard レビューパターン3点あり）
5. Issue #19 本文と私のコメント（path 表記の誤りに注意）

**次に、必ず以下を確認してから着手**:

- 不明点があれば質問（一度に3つまで）。プラン提示前に質問してクリアにする
- 質問がなければ「不明点なし」と明示してから状況把握レポート＋プラン提示へ
- プラン承認後に作業開始（NOTE.md に書いてある通常フロー）

**質問例（参考）**:

- 「Gemini CLI のバージョンは特定のものを想定しますか？」
- 「Gemini DeepResearch 用プロンプト案は私が実行する想定ですか、それともコマンドだけ用意ですか？」
- 「README.md / CLAUDE.md の library/ 反映は本 PR に含める？それとも別 PR？」
