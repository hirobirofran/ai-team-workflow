# Issue #31 起票物 Superpowers レビュー（2026-04-25）

> **実行**: Agent ツール `superpowers:code-reviewer`
> **対象**:
> - GitHub Issue #31 本文（`hirobirofran/ai-team-workflow`）
> - [`handoff/issue-31/20260425_initial_prompt.md`](20260425_initial_prompt.md)
> **実施日**: 2026-04-25
> **結果**: Must 2 件 / Better 4 件 / Optional 2 件
> **修正**: 報告のみ（このレビュー結果を踏まえて別途実施）

---

## 観察ポイント（前提）

レビュー実施前に根拠資料を読み込んだ際に確認した重要な観察:

1. AAR の Issue 起票候補表に B1/B2 が含まれている（[`20260425_aar.md`](../misc/repo_check/20260425_aar.md) L20-21）→ つまり Issue #31 のスコープに B1/B2 が入った根拠は AAR にある
2. ただし、AAR の B1/B2 の出処は **整合性チェック / CLAUDE.md レビューではなく** [`handoff/followup.md`](../followup.md) の「B 取り組み案メモ」由来
3. AAR 学びセクションには「forcing function 待ち」のニュアンスもある

---

## Must（明確に直すべき / scope creep）

### M1. B1/B2 (#5/#6) はエピックタイトルとスコープが乖離している（scope creep）

ユーザー観察「B1/B2 はどこにも指摘ないのに紛れている、別 Issue ではないか?」は正しい。具体的根拠:

- [`20260425_consistency_check.md`](../misc/repo_check/20260425_consistency_check.md) の 🔴9 件 / 🟡7 件のどこにも「NOTE.md にセッション終了時手順を追加」「NOTE.md に運用改善サイクルを追加」は出てこない（出てくるのは「テンプレ汚染修正」「旧表記統一」「リンク切れ」「空ディレクトリ」のみ）
- [`20260425_claude_md_review.md`](../misc/repo_check/20260425_claude_md_review.md) の Must3 / Better4 / Optional2 のどこにも NOTE.md 拡張の指摘は出てこない（vision.md 参照漏れ・commit/push 衝突・閾値分散など、すべて root CLAUDE.md 内の話）
- B1/B2 の出処は明確に [`handoff/followup.md`](../followup.md) L247-273「B（NOTE.md 拡張）の取り組み案メモ」のみ。これは A1〜A3 で本リポ CLAUDE.md に新規追加したパターンを **テンプレに逆輸入** する話で、「2026-04-25 AAR」が指摘した不整合とは因果が違う

エピックタイトル「リポジトリ整合性 + CLAUDE.md 強化（2026-04-25 AAR ベース）」に対して、B1/B2 は「テンプレ拡張（NOTE.md への新規パターン伝播）」で、論題が別物。AAR の起票候補表 (L20-21) に並んでいるのも整理の便宜であって、エピックを束ねる根拠にはならない。さらに「forcing function 待ち（他リポ FB 出てから）」が明記されている (followup.md L273) ので、本エピックに入れると「永遠に閉じないチェックリスト」になる。

**提案**: B1/B2 (#5/#6) を Issue #31 から外し、別 Issue（または起票保留メモのまま）にする。Issue #31 本文の「メタ観察」セクションで「B1/B2 はテンプレ拡張系で別系統、forcing function 待ち」と一行触れるだけでよい。これで Issue #31 は「2026-04-25 AAR の修正タスク」として論題が揃い、サブタスクが 4 件に圧縮されてクローズ可能になる。

### M2. initial_prompt.md L37 の「`CLAUDE.md`（既にロード済み）」前提が Web AI ケースで破綻

[`initial_prompt.md`](20260425_initial_prompt.md) は L14-16 で「Claude.ai Web / Gemini Web / NotebookLM など（ファイル参照不可な AI 用）」用に **本ファイル全文をコピペで貼る**ユースケースを公式サポートしている。しかし L37 で「1. `CLAUDE.md`（既にロード済み）」と書いてある。Web AI には CLAUDE.md は自動ロードされない。

**提案**: L37 を「`CLAUDE.md`（Claude Code 起動時は自動ロード済み。Web AI で使う場合は別途貼付）」に変更、もしくは Web AI 利用時の手順節を別立てして CLAUDE.md 貼付を明示。

---

## Better（質を上げる）

### B1. Issue 本文の「取り組み順の推奨」と initial_prompt.md の表で順序の根拠（循環防止）の表現が弱い

Issue #31 本文 L31 は「P0 (#1) を最初に: `consistency_check.md` 自身が壊れているため、これを修正してから他の整合性チェックを再実施するのが筋（循環防止）」と書いてあり明確。一方 initial_prompt.md L48 表は「**最初**（循環防止）」とだけ書いて理由が省略されている。両ファイル間でロジックの厚みが異なる。

**提案**: initial_prompt.md L48 のセル注釈に「テンプレ自身が壊れたまま他のチェックを走らせると循環する」と一行足すか、表外に注記を出す。AAR L43 の文言を借りればよい。

### B2. checkpoint 運用の参照先が initial_prompt.md にしかない（Issue 本文に記載なし）

initial_prompt.md L61 にはトークン圧迫時の `handoff/issue-31/YYYYMMDD_checkpoint.md` 運用が書いてある。一方 Issue #31 本文には checkpoint 関連の記述が一切ない。新規セッションが Issue 本文だけ見て着手した場合、checkpoint パターンを知らずにトークン枯渇する。

**提案**: Issue #31 本文「担当」節に「セッション分割時は `handoff/issue-31/YYYYMMDD_checkpoint.md` を残してパス」と一行追加。CLAUDE.md の追加義務 (`YYYYMMDD_aar.md` 配置) も Issue 本文に明示するとなおよい。

### B3. AAR ファイル配置義務（`handoff/issue-31/YYYYMMDD_aar.md`）が両ファイルとも未明記

CLAUDE.md L100 で AAR は `handoff/issue-XX/YYYYMMDD_aar.md` に保存義務がある。本エピックは複数サブタスクを跨ぐので、サブタスクごとに AAR を残すか、エピック完了時に 1 本にするかの方針が両ファイルで未定義。後でレビューする際の論拠になるので明示すべき。

**提案**: initial_prompt.md L55「1 件ずつ独立 PR / commit でも OK」の直後に「AAR は **サブタスク単位 or 1 セッション単位**で `handoff/issue-31/YYYYMMDD_aar.md` を残す（同日複数なら `_aar_p0.md` 等の suffix）」と運用を確定させる。

### B4. 「(直近強化) 高」のラベル表記が他と整合しない

Issue #31 本文 L19 だけ「(直近強化) 高」というラベルで、他は「(P0) 高」「(P1) 高」「(P2) 中」「(B1) 中」と P0/P1/P2/B1/B2 の体系。CLAUDE.md レビュー反映だけ別系統のラベルになっており、AI が読み取り順序を判断する際にノイズ。

**提案**: 「(P1.5) 高」または「(C1) 高」（CLAUDE.md レビュー = C 系統）に統一。既に AAR の起票候補表 #5 では「直近強化箇所」と説明的に書かれているだけなので、エピック化に合わせてラベル体系を整える。

---

## Optional（思想として）

### O1. Issue 本文の「メタ観察」が自己参照で弱い

Issue #31 本文 L37「本エピック起票自体が『本リポの運用改善ルールが未熟』ことの露呈であり、同時に『Pre-merge レビュー + 全体整合性チェックの 2 軸並列』パターンの仮説検証としては成功事例」は AAR L29 と重複しており、エピックとして残す価値より AAR / knowledge.md に集約すべき内容。Issue は「何を直すか」に集中させ、振り返りメタは AAR に逃がすほうが Issue 一覧から見たときの可読性が高い。

### O2. forcing function 概念の説明欠落

両ファイルとも「forcing function 待ち」を地の文で使っているが、本リポ運用での定義を読者が遡れる場所がない（followup.md L273 に唯一あり）。M1 で B1/B2 を外せば不要になるが、もし残すなら「forcing function = 他リポでの NOTE.md 実適用 FB を 1 回でも経験すること」と Issue 本文か initial_prompt.md に注記。

---

## 結論

最大の論点は **M1 の scope creep**。ユーザー観察は完全に正しく、B1/B2 は AAR の整合性チェック / CLAUDE.md レビュー結果ではなく followup.md 由来で、エピックタイトル「2026-04-25 AAR ベース」とは因果系統が違う。B1/B2 を切り離せば Issue #31 は閉じやすい単位になる。M2 / B2 / B3 は両ファイルの非対称（片方にしかない情報）で、AI が最初にどちらを読むかで挙動が変わるリスクがある。

**関連ファイル**:

- [`handoff/issue-31/20260425_initial_prompt.md`](20260425_initial_prompt.md)
- [`handoff/misc/repo_check/20260425_aar.md`](../misc/repo_check/20260425_aar.md)
- [`handoff/misc/repo_check/20260425_consistency_check.md`](../misc/repo_check/20260425_consistency_check.md)
- [`handoff/misc/repo_check/20260425_claude_md_review.md`](../misc/repo_check/20260425_claude_md_review.md)
- [`handoff/followup.md`](../followup.md) (L247-273)
- [`CLAUDE.md`](../../CLAUDE.md)
