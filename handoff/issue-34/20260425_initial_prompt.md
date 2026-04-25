# Issue #34 新規セッション起動プロンプト

> **作成**: 2026-04-25
> **対応 Issue**: [#34](https://github.com/hirobirofran/ai-team-workflow/issues/34) [P0] ai-team/templates/ 旧表記統一 + consistency_check.md 汚染修正
> **親 Epic**: [#31](https://github.com/hirobirofran/ai-team-workflow/issues/31)
> **状態**: 即着手可（Epic #31 のサブタスクで最優先＝循環防止のため最初）

## 起動コマンド（コピペで使える）

### Claude Code 用（このファイルを Read させる）

```text
handoff/issue-34/20260425_initial_prompt.md を読んで、不明点があれば聞いて、なければ理解内容と次のアクションを示して
```

---

## 本タスクの性質

- `ai-team/templates/` 配下 3 ファイルの旧表記を新形式に統一する **修正実施タスク**
- **PR 必須**（ai-team/ 配下のため、テンプレなのでレビュー要）
- 機械的な置換が中心だが、`consistency_check.md` 汚染は **テンプレ自身の修正**なので循環防止のため最優先
- 着手時点で他に整合性チェック再実行をしている作業がない前提

## 必読ファイル

1. `CLAUDE.md`（Claude Code 起動時のみ自動ロード。Web AI で使う場合は別途貼付）
2. 本 Issue [#34](https://github.com/hirobirofran/ai-team-workflow/issues/34) 本文
3. **整合性チェック結果**: [`handoff/misc/repo_check/20260425_consistency_check.md`](../misc/repo_check/20260425_consistency_check.md) 🔴 #1, #2, #3
4. **修正対象ファイル**:
   - `ai-team/templates/claude_code_session.md` L20-21, L79
   - `ai-team/templates/aar.md` L4
   - `ai-team/templates/consistency_check.md` L31, L41, L70-71
5. **新形式の正例**: `handoff/issue-XX/YYYYMMDD_*.md`（CLAUDE.md L100-103 で定義）
6. **実行プラン**: [`handoff/issue-31/20260425_plan.md`](../issue-31/20260425_plan.md)（Phase 2 全体像）
7. **プランレビュー**: [`handoff/issue-31/20260425_plan_review.md`](../issue-31/20260425_plan_review.md) M2（再走 verify の根拠）

## 修正方針

1. **`claude_code_session.md`**: `handoff/aar_*.md` / `handoff/plan_*.md` / `handoff/plan_YYYYMMDD.md` を `handoff/issue-XX/YYYYMMDD_aar.md` / `handoff/issue-XX/YYYYMMDD_plan.md` に統一
2. **`aar.md`**: `handoff/aar_YYYYMMDD_タスク名.md` を `handoff/issue-XX/YYYYMMDD_aar.md` に統一
3. **`consistency_check.md`**: 既知の汚染 3 点
   - `docs/plan.md` 単一スコープ → `docs/` 配下全体に拡張
   - 例示が旧表記 → 新形式 `handoff/issue-XX/YYYYMMDD_*.md` に
   - `library/` 等の新要素未考慮 → 現存ディレクトリ全体を対象に（テンプレ持ち出し時に `library/` 等は除外する旨も明記）

## 重要な制約

- `ai-team/` 配下なので **PR 必須**（main 直 push 不可）
- 1 PR にまとめるか 3 ファイルそれぞれ別 PR にするかはユーザー確認（質問例参照）
- コミット粒度はファイル単位で別コミットを推奨（影響範囲分離のため）
- 修正後は **必ず `consistency_check.md` 自身を使って再走 verify**（M2）

## 終了条件（M2 反映: 再走 verify 必須）

1. 上記 3 ファイルの旧表記すべて新形式に統一
2. PR でマージ
3. **修正後の `consistency_check.md` で本リポを再チェック**（Agent ツール `general-purpose` で本リポ全体を走査） → 🔴 #1, #2, #3 が解消したことを確認。新たに発生した 🔴 があれば確認・記録
4. 再チェック結果を `handoff/issue-34/20260425_consistency_recheck.md` に保存（プロンプト全文 + 結果）
5. AAR を `handoff/issue-34/YYYYMMDD_aar.md` に保存
6. 本 Issue を close
7. **親 #31 のチェックリスト P0 を `[x]` に更新**（`gh issue edit 31`、Issue #31 body の該当行を `- [ ]` → `- [x]`）

## 開始手順

1. 不明点があれば質問（一度に 3 つまで）
2. 修正方針を提示してユーザー確認（特に「1 PR or 3 PR」の粒度判断）
3. 承認後にブランチ作成 → 修正 → PR 提出
4. PR マージ後に再走 verify → AAR → close → #31 チェック更新

## 質問例（参考）

- 「`consistency_check.md` 汚染修正と `claude_code_session.md` / `aar.md` 旧表記統一を 1 PR にまとめますか? 別 PR にしますか?」
- 「コミット粒度はファイル単位で 3 commit 推奨ですが、まとめて 1 commit でもいいですか?」
- 「再走 verify は Agent `general-purpose` で本リポ全体走査でいいですか? それとも対象ファイルだけ手動で確認しますか?」
