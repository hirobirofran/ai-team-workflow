# Issue #35 新規セッション起動プロンプト

> **作成**: 2026-04-25
> **対応 Issue**: [#35](https://github.com/hirobirofran/ai-team-workflow/issues/35) [C1] CLAUDE.md レビュー反映（Must 3 + Better 4 + Optional 2）
> **親 Epic**: [#31](https://github.com/hirobirofran/ai-team-workflow/issues/31)
> **状態**: 即着手可。ただし [#34](https://github.com/hirobirofran/ai-team-workflow/issues/34) P0 完了後の方が M2 判断が容易（プラン Phase 3 推奨）

## 起動コマンド（コピペで使える）

### Claude Code 用（このファイルを Read させる）

```text
handoff/issue-35/20260425_initial_prompt.md を読んで、不明点があれば聞いて、なければ理解内容と次のアクションを示して
```

---

## 本タスクの性質

- `CLAUDE.md`（root）への運用ルール強化（Must 3 + Better 4 + Optional 2 の 9 項目）
- **main 直 push 可**（root 配下、PR 不要）
- 設計判断を伴う中心ファイルの修正なので、Must / Better / Optional は **別 commit 推奨**

## 必読ファイル

1. `CLAUDE.md`（Claude Code 起動時のみ自動ロード。Web AI で使う場合は別途貼付）
2. 本 Issue [#35](https://github.com/hirobirofran/ai-team-workflow/issues/35) 本文
3. **CLAUDE.md レビュー結果**: [`handoff/misc/repo_check/20260425_claude_md_review.md`](../misc/repo_check/20260425_claude_md_review.md) 全文（Must 3 + Better 4 + Optional 2 の詳細指摘）
4. **修正対象ファイル**: `CLAUDE.md`（root）
5. `docs/vision.md`（M1 で参照を追加する対象）
6. **実行プラン**: [`handoff/issue-31/20260425_plan.md`](../issue-31/20260425_plan.md)（Phase 2 全体像）
7. **依存先**: [#34](https://github.com/hirobirofran/ai-team-workflow/issues/34) P0（M2 判断のため先行推奨。完了状況を `gh issue view 34` で確認）

## 修正方針（review ファイル参照、要約）

**Must 3 件（必須反映）**:

- **M1**: `docs/vision.md` 参照を構造図 + 「セッション開始時 Step 2」の読み物リストに追加（初回・方針議論時のみ）
- **M2**: 整合性チェック参照先（`ai-team/templates/consistency_check.md`）の存在確認 → #34 P0 完了で汚染修正済みなら参照妥当性 OK、未完了なら状態に応じて判断
- **M3**: 「セッション終了時の手順」と「作業ルール」で commit/push の扱いが衝突 → 終了時手順側に「内容を見せて承認後 push」を 1 行明示

**Better 4 件**: A/B 分岐の判定順序明示 / 3 件以上 consolidate 閾値の集約 / 永続化先一覧の重複解消 / A1「Step 1〜4 は実行しない」表現緩和

**Optional 2 件**: 業務手順書観点の前文 1 行 / A3 が NOTE.md に出さない理由を冒頭に注記

## 重要な制約

- CLAUDE.md（root）は **main 直 push 可**だが、大きな変更はユーザー確認推奨
- Must / Better / Optional は別 commit 推奨（影響範囲分離）
- ユーザーは Better/Optional の採否判断に意見を持ちたい可能性あり → 採否について確認推奨
- review ファイルの **不採用判断は理由明記**（[#31 の B1 不採用例](../issue-31/20260425_plan.md) 参照）

## 依存

- M2 は #34 P0（`consistency_check.md` 汚染修正）完了後の方が判断しやすい。**推奨着手順**: #34 → #35
- ただし M1 / M3 / Better / Optional は #34 完了を待たなくても着手可

## 終了条件

1. Must 3 件すべて反映
2. Better 4 件 / Optional 2 件は採否判断のうえ反映（不採用は理由明記）
3. AAR を `handoff/issue-35/YYYYMMDD_aar.md` に保存
4. 本 Issue を close
5. **親 #31 のチェックリスト C1 を `[x]` に更新**

## 開始手順

1. #34 P0 の完了状況を確認（`gh issue view 34`）
2. 不明点があれば質問（一度に 3 つまで）
3. 修正方針を提示してユーザー確認（特に Better/Optional の採否、コミット粒度）
4. 承認後に修正 → 1〜3 commit & main 直 push
5. AAR → close → #31 チェック更新

## 質問例（参考）

- 「Must 3 件は別 commit に分けますか? 1 commit でいいですか?」
- 「Better 4 件のうち、採用したい/不採用にしたい項目はありますか?」
- 「Optional 2 件は反映しますか? それとも knowledge.md / followup.md に逃がしますか?」
