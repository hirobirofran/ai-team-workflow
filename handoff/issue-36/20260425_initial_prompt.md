# Issue #36 新規セッション起動プロンプト

> **作成**: 2026-04-25
> **対応 Issue**: [#36](https://github.com/hirobirofran/ai-team-workflow/issues/36) [P2] knowledge.md 旧表記 + handoff/issue-19/ 処遇 + handoff/misc/ サブ構造記載
> **親 Epic**: [#31](https://github.com/hirobirofran/ai-team-workflow/issues/31)
> **状態**: 即着手可（プラン Phase 3 推奨着手順では P0 → C1 の後）

## 起動コマンド（コピペで使える）

### Claude Code 用（このファイルを Read させる）

```text
handoff/issue-36/20260425_initial_prompt.md を読んで、不明点があれば聞いて、なければ理解内容と次のアクションを示して
```

---

## 本タスクの性質

整合性チェックで指摘された残りの軽量整理を 3 つ統合実施:

1. `docs/knowledge.md` 旧表記 4 箇所更新（機械的）
2. `handoff/issue-19/` 空ディレクトリ処遇（判断要、削除 or `.gitkeep` or 残置）
3. `handoff/misc/feedback/` `handoff/misc/repo_check/` サブ構造を `CLAUDE.md` / `ai-team/NOTE.md` に記載

PR 範囲が混在: 1 と 2、3 のうち CLAUDE.md は **main 直 push 可**、3 のうち NOTE.md は **PR 必須**（`ai-team/` 配下）。

## O1 反映: 分割再考の余地

**着手時に分割が筋なら新セッションで分割提案して構わない**。CLAUDE.md / NOTE.md の PR 範囲が混在しており、機械的部分（main 直 push）と PR 必須部分を別 Issue/PR で分けると分業効率が上がる可能性がある（B1/B2 = #32/#33 と類似構造）。

判断の目安:
- 着手者が 1 セッションで全 3 件を捌けそう → 分割不要、本 Issue で完結
- NOTE.md 追記の PR レビューサイクルが重そう → 機械的部分（CLAUDE.md 追記 + knowledge.md + issue-19/）と NOTE.md 追記で分割

分割する場合は派生 Issue を本 Issue 配下にぶら下げてから本 Issue を close（または親 #31 の関連 Issue として直接ぶら下げる）。

## 必読ファイル

1. `CLAUDE.md`（Claude Code 起動時のみ自動ロード。Web AI で使う場合は別途貼付）
2. 本 Issue [#36](https://github.com/hirobirofran/ai-team-workflow/issues/36) 本文
3. **整合性チェック結果**: [`handoff/misc/repo_check/20260425_consistency_check.md`](../misc/repo_check/20260425_consistency_check.md) 🔴 #4, #8, #9
4. **修正対象**:
   - `docs/knowledge.md` L66, L82, L144, L643（旧表記 4 箇所）
   - `handoff/issue-19/`（空ディレクトリ、Issue #28 が PoC 兼任のため実体は #28 側）
   - `CLAUDE.md`（misc サブ構造記載追加）
   - `ai-team/NOTE.md`（misc サブ構造記載追加、PR 必須）
5. **実行プラン**: [`handoff/issue-31/20260425_plan.md`](../issue-31/20260425_plan.md)（Phase 2、O1 反映）
6. **類似分割実例**: [#32](https://github.com/hirobirofran/ai-team-workflow/issues/32) / [#33](https://github.com/hirobirofran/ai-team-workflow/issues/33) initial_prompt（B1/B2 = scope creep として #31 から切り出し済）

## 修正方針

### 1. `docs/knowledge.md` 旧表記更新（機械的）

L66, L82, L144, L643 の `handoff/plan_YYYYMMDD.md` 等を `handoff/issue-XX/YYYYMMDD_plan.md` 形式に。Grep で他の旧表記がないかも追加確認推奨。

### 2. `handoff/issue-19/` 処遇

選択肢:
- (a) ディレクトリ削除（Issue #19 の実体が #28 側に集約済なら削除妥当）
- (b) `.gitkeep` を置いて空ディレクトリの存在意図を明確化
- (c) Issue #19 を close した上でディレクトリ削除（#19 の状態確認が要る）

着手時に Issue #19 の状態を確認 (`gh issue view 19`) してから判断。

### 3. `handoff/misc/` サブ構造記載

- `CLAUDE.md` の構造図 / 説明節に「`handoff/misc/feedback/`（壁打ちフィードバック）」「`handoff/misc/repo_check/`（リポ整合性チェック結果）」を追加
- `ai-team/NOTE.md` の構造図にも同様の記載（ただし `library/` のように「リポ固有なので持ち込み先で個別判断」旨の注記が要るか検討）

## 重要な制約

- `docs/knowledge.md` 更新と `handoff/issue-19/` 処遇は **main 直 push 可**
- `CLAUDE.md` への misc サブ構造記載も **main 直 push 可**（root 配下）
- `ai-team/NOTE.md` への misc サブ構造記載は **PR 必須**（ai-team/ 配下）
- 1 Issue で全部やる場合は commit を分けて影響範囲を分離（推奨: 機械的部分 1 commit + NOTE.md 追記 1 PR）

## 終了条件

1. 上記 3 件すべて対応（または分割判断後に各 Issue/PR で対応）
2. NOTE.md 追記は PR でマージ、それ以外は main 直 push
3. AAR を `handoff/issue-36/YYYYMMDD_aar.md` に保存
4. 本 Issue を close（分割した場合は派生 Issue が全 close したことを確認してから本 Issue を close）
5. **親 #31 のチェックリスト P2 を `[x]` に更新**

## 開始手順

1. 不明点があれば質問（一度に 3 つまで）
2. **分割判断**: 1 Issue で全 3 件を本セッションで捌くか、NOTE.md 追記を別 Issue/PR に切り出すか提示
3. 着手方針を提示してユーザー確認
4. 承認後に修正 → commit & push（または PR）
5. AAR → close → #31 チェック更新

## 質問例（参考）

- 「3 件をまとめて 1 Issue で対応しますか? それとも NOTE.md 追記だけ別 Issue/PR に切り出しますか?（O1 反映）」
- 「`handoff/issue-19/` は (a) 削除 / (b) `.gitkeep` / (c) Issue #19 close 後削除 のどれにしますか? Issue #19 の現状確認結果を踏まえて判断します」
- 「`ai-team/NOTE.md` の misc サブ構造記載は『リポ固有なので持ち込み先で個別判断』旨の注記を入れますか?（library/ と同じ扱い）」
