# 整合性チェック結果（リポジトリ全体、2026-04-25）

> **実行**: Agent ツール `general-purpose`
> **対象**: ai-team-workflow リポジトリ全体
> **実施日**: 2026-04-25
> **所要**: 約 103 秒
> **結果**: 🔴 9 件 / 🟡 7 件 / 🟢 5 領域問題なし
> **修正**: 今回はしない（AAR で判断）
> **参考テンプレ**: `ai-team/templates/consistency_check.md`（汚染ありのため一部補正して使用）

---

## 実行プロンプト（再現性検証用、全文）

```text
ai-team-workflow リポジトリ全体の **整合性チェックを実施** してください。**修正はしないでください、報告のみ**。

## 参考テンプレート（ただし汚染あり）

`ai-team/templates/consistency_check.md` を参考にしつつ、以下の汚染を補正してください：

- **「docs ↔ 実態の乖離」が `docs/plan.md` 単一ファイル限定スコープ** → `docs/` 配下全体に拡張
- **例示が旧表記**（`handoff/aar_*.md` 旧形式）→ 現状の `handoff/issue-XX/YYYYMMDD_*.md` 形式に合わせる
- **`library/` 等の新要素未考慮** → 現存する全ディレクトリ・ファイルを対象に

## チェック項目（テンプレ補正済）

1. **未記載ファイル・フォルダ検知**: `README.md` / `ai-team/README.md` / `CLAUDE.md` の構造図に載っていないファイル・フォルダ
2. **docs ↔ 実態の乖離**（`docs/` 配下全体）: 記載と実態の不一致（plan.md のチェックリストだけでなく、knowledge.md / vision.md 等も対象）
3. **ファイル間の矛盾**: 同じ概念の説明が複数ファイルにあるとき、内容食い違い（フォルダ構成・handoff パス表記・AI の役割等）
4. **リンク切れ**: 相対パスリンク・`@ファイル名` 参照が実在するか
5. **テンプレート ↔ 手順書の乖離**: `ai-team/README.md` と `ai-team/templates/` の対応
6. **新要素の反映漏れ**: `library/` など最近追加された要素が各構造図・説明に反映されているか
7. **handoff 規約の遵守**: `handoff/issue-XX/YYYYMMDD_*.md` 命名規約が守られているか

## 出力フォーマット

[テンプレ準拠の表形式 + 修正候補サマリー]

「修正してよいですか？」とは聞かないでください（今回は所見のみ取得）。

長すぎないよう要点に絞ってください。
```

---

## チェック結果（全文）

**実行日**: 2026-04-25
**対象リポジトリ**: ai-team-workflow

### 🔴 問題あり（要対応）

| # | 種別 | 場所 | 内容 |
|---|------|------|------|
| 1 | テンプレ ↔ 実態の乖離 | `ai-team/templates/claude_code_session.md` L20-21, L79 | 旧表記 `handoff/aar_*.md` / `handoff/plan_*.md` / `handoff/plan_YYYYMMDD.md` のまま。CLAUDE.md・NOTE.md・README.md は `handoff/issue-XX/YYYYMMDD_*.md` に更新済みだがここだけ旧形式。テンプレ持ち出し先で誤った命名が伝播する |
| 2 | テンプレ ↔ 実態の乖離 | `ai-team/templates/aar.md` L4 | 同上、`handoff/aar_YYYYMMDD_タスク名.md` の旧表記 |
| 3 | テンプレ汚染（自己参照） | `ai-team/templates/consistency_check.md` L31, L41, L70-71 | 既知の汚染（今回ユーザーが指摘した 3 点）。`docs/plan.md` 単一スコープ／`handoff/aar_*.md` 旧表記の例示／`library/` 等の新要素未考慮。テンプレ自身が修正対象 |
| 4 | docs ↔ 実態の乖離 | `docs/knowledge.md` L66, L82, L144, L643 | 「初期プランを `handoff/plan_YYYYMMDD.md` に保存」など旧表記が 4 箇所残存。実態は `handoff/issue-XX/YYYYMMDD_plan.md` |
| 5 | リンク切れ（パス誤り） | `docs/plan.md` L74-76 | `handoff/misc/20260422_claude_ai_feedback.md` 等 3 件は実体が `handoff/misc/feedback/` 配下にある（`handoff/misc/feedback/20260422_claude_ai_feedback.md` 等）。階層がひとつ抜けている |
| 6 | リンク切れ（URL タイポ） | `handoff/followup.md` L75, L118, L154, L160 | リポジトリ名が `eam-workflow` になっているリンクが 4 箇所（正: `ai-team-workflow`）。Issue #20 / #21 / #28 のリンクが全て 404 になる |
| 7 | 未記載ディレクトリ | `library/`（`ai-team/README.md` 内） | `ai-team/README.md` のファイル構成図は `ai-team/` 配下のみ説明しているのは妥当だが、ルート README.md と CLAUDE.md には `library/` が載っているのに対し、本リポ運用全体を説明する文書群の一貫性として、`ai-team/README.md` の冒頭・フローに「`library/` はリポジトリ固有なのでテンプレに含めない」旨の注記がない（テンプレ汚染回避ルールの明文化が不足） |
| 8 | 未記載ファイル | `handoff/misc/repo_check/`, `handoff/misc/feedback/` | CLAUDE.md L19 は「`handoff/misc/`：Issue に紐づかない作業」と記載するのみで、現状 `misc/feedback/`・`misc/repo_check/` のサブディレクトリ運用が暗黙的。NOTE.md L83 も「`misc/` Issue に紐づかない作業」のみ。慣習化したサブディレクトリの説明がない |
| 9 | handoff 規約違反（空ディレクトリ） | `handoff/issue-19/` | 空ディレクトリ。Issue #19 用に切られたが何も入っていない（`.gitkeep` もなし）。Issue #28 が PoC を兼ねたため実体は `handoff/issue-28/` 側に集約された経緯はあるが、空ディレクトリの存在意図が不明 |

### 🟡 軽微（できれば対応）

| # | 種別 | 場所 | 内容 |
|---|------|------|------|
| 1 | docs ↔ 実態の乖離 | `docs/plan.md` L18 | `[ ] Markdown 改行の修正（プレビューで改行しない箇所）` が未チェックだが、現行ファイル群に該当する未修正箇所が見当たらない（消化済み or 作業完了の可能性。要棚卸し） |
| 2 | docs ↔ 実態の乖離 | `docs/plan.md` L34 | `[ ] Claude Code レート上限時のコパイロットフォールバック手順` 未対応。knowledge.md L641-644 でも「未解決」と記載されており実態と整合しているが、対応する受け皿（Issue 番号）への明示的リンクがない |
| 3 | ファイル間矛盾 | `docs/plan.md` 末尾「既知の課題」表 vs `docs/knowledge.md` 多数 | plan.md の表は v0.1 ベースのまま簡素。knowledge.md にはモデルディスパッチャパターン・前提疑い問題など多数の新知見があるが、plan.md 既知課題リストに反映されていない |
| 4 | 未記載ファイル | `handoff/followup.md` | CLAUDE.md・library/index.md には説明あり。NOTE.md / ai-team/README.md・ルート README.md の構造図には登場しない（このリポ固有なので NOTE.md には載せなくてよいが、ルート README.md には言及があってもよい） |
| 5 | テンプレ ↔ 手順書の乖離 | `ai-team/README.md` Step 0 と `ai-team/templates/researcher.md` | README.md は明示的に「Claude Code で下書きブラッシュアップ」を勧めるが、researcher.md（先頭 50 行のみ確認）は「事前調査結果を整理」が主目的の構造のまま。手順 1 「Claude Code でブラッシュアップ」が researcher.md 側にも明示されているか要再確認（README.md の指示と templates の指示の対称性） |
| 6 | リンク参照（スタイル） | `docs/plan.md` 完了済み項目の表記揺れ | 「次回セッションで整理するタスク」の `[x]` 完了項目が現役のチェックリストに紛れ込んでいる。完了済みは別セクションに移すかストライクスルー化すると参照性が上がる |
| 7 | 未記載ファイル | `.markdownlint.json` | ルート構成図（README.md L10-18, CLAUDE.md L11-21）に記載なし。dotfile なので妥当だが、リント運用の説明が docs/ にも一切ない |

### 🟢 問題なし

- `ai-team/templates/` のファイル全 8 種は `ai-team/README.md` のファイル構成図（L62-74）に過不足なく記載
- `CLAUDE.md` ↔ `ai-team/NOTE.md` の `handoff/issue-XX/YYYYMMDD_*.md` 命名規約は一致
- `library/` ↔ `library/index.md` ↔ `library/ai-tooling-ecosystem.md` の三者間で命名・運用ルール（pull 型・PR 経由・カテゴリ）に矛盾なし
- ルート README.md と CLAUDE.md の構造図（top-level）は `library/` まで含めて整合
- `handoff/issue-28/` 配下のファイル群（plan/aar/research/initial_prompt/plan_review/gemini_review/superpowers_review）は命名規約 `YYYYMMDD_*.md` を完全遵守

### 📋 修正候補サマリー（優先度順）

**P0（テンプレ汚染を伝播させない・即時）**

1. `ai-team/templates/claude_code_session.md` の旧表記を新形式に統一（🔴 #1）
2. `ai-team/templates/aar.md` 同上（🔴 #2）
3. `ai-team/templates/consistency_check.md` の汚染 3 点を修正（🔴 #3）

**P1（リンク切れの実害）**

4. `handoff/followup.md` の `eam-workflow` タイポ 4 箇所訂正（🔴 #6、Issue リンク全て 404）
5. `docs/plan.md` L74-76 のパス階層補正（🔴 #5）

**P2（ドキュメント整合性）**

6. `docs/knowledge.md` 旧表記 4 箇所更新（🔴 #4）
7. `handoff/issue-19/` 空ディレクトリ処遇判断（🔴 #9）
8. `handoff/misc/feedback/` `handoff/misc/repo_check/` サブディレクトリ運用を CLAUDE.md / NOTE.md に追記（🔴 #8）

**P3（軽微）**

9. `docs/plan.md` フェーズ1チェックリスト棚卸し（🟡 #1, #2, #6）
10. `ai-team/README.md` に「`library/` 等のリポ固有要素はテンプレに含めない」旨の注記追加（🔴 #7）

---

**所感**: P0 の 3 件はテンプレ持ち出し先で命名規約が壊れる実害があり最優先。P1 の 2 件は内部参照リンク切れ。P2 以下は時間ができたとき。`consistency_check.md` 自体の汚染（🔴 #3）が `ai-team/` 配下なので PR 必須なのが運用上やや重いが、テンプレ汚染を放置するメリットはない。
