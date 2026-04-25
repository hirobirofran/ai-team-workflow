# Issue #30 新規セッション起動プロンプト

> **作成**: 2026-04-25
> **対応 Issue**: [#30](https://github.com/hirobirofran/ai-team-workflow/issues/30) ai-team/ テンプレに「最新情報ウォッチ機能」が欲しい
> **関連 Issue**: [#19](https://github.com/hirobirofran/ai-team-workflow/issues/19) library/ 新設、[#28](https://github.com/hirobirofran/ai-team-workflow/issues/28) library/ 運用 PoC、[#21](https://github.com/hirobirofran/ai-team-workflow/issues/21) 自己ナレッジループ
> **想定使用方法**:
>
> - **Claude Code**: ユーザーが新セッションで「このファイルを読んで、不明点があれば聞いて、なければ理解内容と次のアクションを示して」と指示 → 本ファイルを直接 Read
> - **Claude.ai Web / Gemini など**: 本ファイル全文をコピペして貼付（ファイル添付は読まれないことが多いため）

---

ai-team-workflow リポジトリで [Issue #30](https://github.com/hirobirofran/ai-team-workflow/issues/30) の調査・議論作業を行います。

このリポジトリ自体の運用ルールは CLAUDE.md を参照（既に自動ロード済み）。
リサーチ手順は @ai-team/templates/researcher.md を参照してください。

## 本タスクの性質

**メインタスク**: 「テンプレに RSS フィード相当の最新情報ウォッチ機能が欲しい」という希望に対し、HOW を **議論と整理で詰める**。

**重要な特性**:

- Issue #30 本文は **問題提起のみ**。具体策（NOTE.md にどう書く / カテゴリどうする等）は決まっていない
- リサーチ駆動ではなく **議論駆動**。一次情報の裏取りより、既存資産の整理と方向性合意が先
- 「完璧な仕様を一発で書く」より「動く軽い案で試す」方向（Issue #28 PoC で得た教訓を逆方向に活かす）

## 検討の出発点（2 方向の選択肢 + α）

### A. personal-counselor の library/ をテンプレ化する

**事実**: 別リポジトリ `personal-counselor` で library/ が **軽量に先行運用**されている（2026-04-25 時点）。
取得方法（CLAUDE.md グローバルルールの base64 -d パイプ経由）:

```bash
gh api "repos/hirobirofran/personal-counselor/contents/library/index.md" --jq '.content' | base64 -d
```

**特徴（取得済み）**:

- 6 項目の箇条書き運用ルール（書き手・更新タイミング・粒度・URL 必須・関連度ラベル・相互リンク）
- ◎/○/△/× の関連度ラベル
- docs/ との対比表で目的を明確化
- 1 トピック = 1 ファイル、各ファイル内のフォーマットは柔軟

**この方式をテンプレ化する案**: `ai-team/library/`（テンプレ配下）にスケルトンを置き、cp で持ち込まれた他リポジトリで運用される形にする（**持ち込み先のルートを汚染しない**）。

### B. ai-team-workflow 側の library/ を軽量化リファクタする

**事実**: 本リポの `library/index.md` は Issue #28 PoC で作成されたが、**運用ルールが厚め**（pull 型 / ログ→原則 2 段階 / PR 経由 / 熟成タイミング 等）。実運用駆動でなく PoC 設計駆動だったため。

**この方向**: personal-counselor 方式（6 項目軽量規約）に合わせて `library/index.md` を軽量化リファクタしてからテンプレ化する。「逆に学ぶ」アプローチ。

### C. その他

A と B の中間案、または全く別の方向もありえる。議論で詰める。

## このリポジトリのフォルダ規約（再確認）

| 場所 | 役割 |
|------|------|
| `ai-team/` | 他リポジトリ向けテンプレ（クリーンに保つ・**ルート汚染回避**） |
| `docs/` | このリポジトリの設計書 |
| `handoff/` | このリポジトリの作業ファイル |
| `library/` | このリポジトリ固有の生物情報スナップショット |

**重要**: テンプレ側に library/ 概念を組み込む際、**持ち込み先リポジトリのルートを汚染しない**設計が前提（library/ は `ai-team/` 配下に閉じる）。

## 関連する既存議論・成果物（読むことを推奨）

1. CLAUDE.md（既にロード済み）
2. `ai-team/templates/researcher.md`（リサーチ手順）
3. `docs/knowledge.md` の以下の節:
   - 「リサーチは『オプション』ではなく最重要フェーズ」
   - 「別セッション Opus によるプランレビューが効いた（実地観察 2026-04-25）」
   - 「『前提を疑う』が AI 単体で実施しづらい（実地観察 2026-04-25 #30 起票）」（**前提崩れ ①〜⑦ の実例集、必読**）
4. `handoff/followup.md` の以下のエントリ:
   - 「#28 PoC からの follow-up」
   - 「『前提疑い』外部委譲手段の試運転候補」
5. `handoff/issue-28/` の成果物:
   - `20260425_aar.md`（PoC 観点 4 軸所見）
   - `20260425_gemini_review.md`（CWD ファイル参照課題）
   - `20260425_superpowers_review.md`（誘導バイアス課題）
6. このリポの `library/index.md`（Issue #28 PoC 版）
7. personal-counselor の library/index.md（上記 base64 -d パイプで取得）
8. Issue #30 本文と関連 Issue #19, #21, #28

## 重要な制約

- **Opus 全振り想定**。同一セッション内でモデル切替しない（参照: `docs/knowledge.md` の 2026-04-25 ログ）
- **議論駆動なので、リサーチに深入りしない**。一次情報での裏取りは A / B / C のいずれかを選んでから個別実施
- トークン圧迫の兆候があれば（残 40% 下回り）`handoff/issue-30/YYYYMMDD_checkpoint.md` にスナップショット保存して新セッションへ
- ブランチを切って作業（`feature/issue-30-template-watch-feature` 等）、最後に PR 作成（マージは私が判断）
- **`ai-team/` 配下を変更する場合は PR 必須**（CLAUDE.md ルール）
- **「Issue は問題提起だけ」原則を尊重**: PR を出すのは具体策が議論で固まってから。途中で「こういう実装どうですか」と先回りしない

## 終了条件

- [ ] 「テンプレに RSS ウォッチ機能を組み込む方針」が決まっている（A / B / C のいずれか、または別案）
- [ ] 方針に従って `ai-team/` 配下に必要な変更が入っている
- [ ] 持ち込み先のルートを汚染しない設計になっている
- [ ] `handoff/issue-30/` に plan.md / aar.md（最低限）が揃っている
- [ ] PR が作成されている
- [ ] AAR で Issue #30 自体への所感（軽量化方針が機能したか等）が記録されている

## 開始手順

**まず以下を確認してから着手**:

- 不明点があれば質問（一度に 3 つまで）。プラン提示前に質問してクリアにする
- 質問がなければ「不明点なし」と明示してから状況把握レポート＋プラン提示へ
- プラン承認後に作業開始（NOTE.md に書いてある通常フロー）

**質問例（参考）**:

- 「A / B / C のどれを推奨枠としますか？ それとも全部議論材料として並べますか？」
- 「軽量化リファクタ（B 案）を選ぶ場合、本リポの library/ 既存エントリ（`ai-tooling-ecosystem.md` の Gemini CLI DR の節）はどう扱いますか？ 保持・改稿・削除？」
- 「他リポジトリへの持ち込み（cp）時の挙動は本 Issue で検証しますか？ それとも別 Issue ですか？」
