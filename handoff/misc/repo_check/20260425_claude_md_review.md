# CLAUDE.md レビュー結果（Superpowers、2026-04-25）

> **実行**: Agent ツール `superpowers:code-reviewer`
> **対象**: `CLAUDE.md`（root）
> **実施日**: 2026-04-25
> **所要**: 約 36 秒
> **結果**: Must 3 件 / Better 4 件 / Optional 2 件
> **修正**: 今回はしない（AAR で判断）

---

## 実行プロンプト（再現性検証用、全文）

```text
ai-team-workflow リポジトリの **`CLAUDE.md`（root）** を実装前レビューしてください。修正はしない、報告のみ。

## 背景

- AI チームワークフロー templates + ナレッジリポジトリ。コードでなくドキュメントとプロンプトテンプレートが主成果物
- `ai-team/NOTE.md` は他リポへ持ち出すテンプレ、`ai-team/` 配下は変更時 PR 必須
- `handoff/followup.md` は Claude Code 自己向け低摩擦メモ（PR 通さず main 直 push）
- `docs/knowledge.md` は長期ナレッジ
- 今日 A1〜A3 で CLAUDE.md に 3 セクション追加直後（commit 17d9559 セッション終了時の手順 / f6ad73e 運用改善サイクル / cfeb58c セッション開始時 A/B 分岐）

## レビュー観点

- 内容の妥当性（事実と推論の区別、論理飛躍がないか）
- 抜けている考慮事項
- 既存ルールとの整合性（CLAUDE.md 内の他セクション、`ai-team/NOTE.md`、`docs/knowledge.md`、`handoff/followup.md` との整合）
- 表記・構造の改善余地
- **AI が順次実行することを前提とした記述順** になっているか（人間が後で読み返す前提と混同していないか）
- 既存運用ルールとの重複・冗長

## 回答フォーマット

- **Must**: 明確に直すべき / 危険な誤りがある
- **Better**: 質を上げる
- **Optional**: 思想として深掘り

300〜500 語程度で簡潔に。
```

---

## レビュー結果（全文）

### 良いところ

「セッション開始時」A/B 分岐、「セッション終了時の手順」、「運用改善サイクル」の3点は **Pull 型 / 開始・終了の台本化 / 改善閾値の三段** という構造で、AI が逐次実行する業務手順書として適切な並びになっています。followup.md の意図と整合し、`ai-team/NOTE.md` との役割分担（テンプレ vs 自リポ運用）も冒頭で明示されている。

### Must（明確に直すべき）

1. **`docs/vision.md` への参照が抜けている**。`docs/` 直下に `knowledge.md` `plan.md` `vision.md` があるのに、構造図と「セッション開始時 Step 2」では `vision.md` が無視されている。最低でも構造図の `docs/` 行に列挙し、Step 2 の読み物リストに「初回・方針議論時のみ vision.md」を加えるべき。

2. **「整合性チェック」の参照先が壊れている疑い**。L32 の `ai-team/templates/consistency_check.md` が実在するか未確認。`ai-team/templates/` 配下に該当物が無ければ Must（リンク切れの作業手順は AI が空振り実行する）。実在しないなら、参照そのものを削除するか整備タスクを Issue 化。

3. **「セッション終了時の手順」と「作業ルール」で commit/push の扱いが衝突**。
   - L28 作業ルール: 「コミット・プッシュ・PR は確認してから」（ユーザー承認必須）
   - L95 終了時手順: 「`git add` → `git commit` → `git push origin main` を完了させる（push 漏れは…絶対）」
   AI が逐次読むと「終了時は確認スキップで push してよい」と解釈しうる。終了時手順側にも「内容を見せて承認後に push」を1行明示すべき。

### Better（質を上げる）

4. **A/B 分岐の判定順序が AI 視点で曖昧**。L68「ユーザーから挨拶や作業開始の合図があったら、まず起動契機を判別」と書いてあるが、判別の **入力（ユーザー発話に `initial_prompt.md` の文字列が含まれるか）** が明示されていない。「ユーザー発話に `initial_prompt.md` の言及が無ければ B」と一行加えると分岐が決定的になる。

5. **「3件以上で consolidate」の閾値が二箇所に分散**（L29 作業ルール、L92 終了時、L106 改善サイクル）。閾値は1箇所に集約し、他は参照だけにする方が autovacuum 自身の精神に沿う（既に followup.md の「3件以上ルール」と重複している）。

6. **「運用改善サイクル」の Step 1 と「セッション終了時の手順」Step 1 の永続化先一覧がほぼ同文の重複**。L90-93 と L104-105 を一方に寄せて参照。

7. **A1（initial_prompt 経由）の「Step 1〜4 は実行しない」が過剰拘束気味**。`initial_prompt.md` 内に「先に gh issue list は走らせて」等の前提が抜けるケースがある。「initial_prompt.md が明示しない限り Step 1〜4 はスキップ」と表現を緩めると安全。

### Optional（思想として）

8. **「業務手順書」観点（followup.md L275-277）を CLAUDE.md 自身の前文に1行入れる**と、後続セクションが「台本である」という枠組みで読める。今は「行動規範」と書かれているだけで、Pull 型・先回り禁止という設計意図が暗黙。

9. **A3 を NOTE.md に出さない判断**は followup.md L270 で言語化済み。CLAUDE.md 冒頭の「ai-team/NOTE.md は他リポ用」注記の直後に「A/B 分岐は本リポ固有」と一言添えると、将来の自分が NOTE.md に逆輸入しようとする事故を防げる。

---

**関連ファイル**: `CLAUDE.md` / `ai-team/NOTE.md` / `handoff/followup.md` / `docs/` (vision.md, knowledge.md, plan.md)
