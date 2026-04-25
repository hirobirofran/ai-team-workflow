# Issue #31 新規セッション起動プロンプト

> **作成**: 2026-04-25
> **対応 Issue**: [#31](https://github.com/hirobirofran/ai-team-workflow/issues/31) [Epic] リポジトリ整合性 + CLAUDE.md 強化（2026-04-25 AAR ベース）
> **想定使用方法**:
>
> - **Claude Code**: ユーザーが新セッションで「このファイルを読んで、不明点があれば聞いて、なければ理解内容と次のアクションを示して」と指示 → 本ファイルを直接 Read
> - **Claude.ai Web / Gemini など**: 本ファイル全文をコピペして貼付

---

ai-team-workflow リポジトリの [Issue #31](https://github.com/hirobirofran/ai-team-workflow/issues/31)（エピック）に対応します。

このリポジトリ自体の運用ルールは `CLAUDE.md` を参照（既に自動ロード済み）。

## 本タスクの性質

**エピックタスク**: リポジトリ整合性チェック + CLAUDE.md レビューの結果から派生した 6 件のサブタスクをまとめて対応する。

**重要な特性**:

- 整合性チェックと CLAUDE.md レビューは **既に実施済み**（プロンプト含めて結果ファイルに保存済）
- 各サブタスクは **修正実施タスク**（議論駆動ではなく実装駆動）
- ただし `ai-team/` 配下の変更は **PR 必須**（テンプレなのでレビュー要）
- それ以外（`docs/` `handoff/` `library/`）は main 直接 push 可

## 必読ファイル（全部読んでから着手）

1. `CLAUDE.md`（既にロード済み）
2. **AAR**: [`handoff/misc/repo_check/20260425_aar.md`](../misc/repo_check/20260425_aar.md) ← 全体把握はここから
3. **CLAUDE.md レビュー結果**: [`handoff/misc/repo_check/20260425_claude_md_review.md`](../misc/repo_check/20260425_claude_md_review.md) ← サブタスク 3 の詳細
4. **整合性チェック結果**: [`handoff/misc/repo_check/20260425_consistency_check.md`](../misc/repo_check/20260425_consistency_check.md) ← サブタスク 1, 2, 4 の詳細
5. **B 取り組み案メモ**: `handoff/followup.md` 「B（NOTE.md 拡張）の取り組み案メモ」 ← サブタスク 5, 6 の詳細
6. Issue #31 本文（チェックリスト確認）

## 取り組み順（エピック本文より）

| # | サブタスク | 緊急度 | 順序 |
|---|---|---|---|
| 1 | (P0) `ai-team/templates/` 旧表記統一 + `consistency_check.md` 汚染修正 | **高** | **最初**（循環防止）|
| 2 | (P1) `handoff/followup.md` タイポ + `docs/plan.md` パス修正 | **高** | 並行可 |
| 3 | CLAUDE.md レビュー反映（vision.md 参照 + commit/push 衝突解消 等） | 高 | 上 2 件後 |
| 4 | (P2) `docs/knowledge.md` 旧表記 + `handoff/issue-19/` 処遇 + `handoff/misc/` サブ構造記載 | 中 | 余裕あれば |
| 5 | (B1) NOTE.md 終了時手順テンプレ追加 | 中 | forcing function 待ち（他リポ FB 後）|
| 6 | (B2) NOTE.md 運用改善サイクルテンプレ追加 | 中 | 同上 |

**1 セッションで全部やる必要はない**。サイズ大きいので、1 件ずつ独立 PR / commit でも OK。

## 重要な制約

- **ユーザー指示「修正は分割実施」**: 1 セッションで全 6 件は重い。1〜2 件ごとに区切って AAR を残しながら進める
- **`ai-team/` 配下は PR 必須**（サブタスク 1, 5, 6）。`docs/` `handoff/` は main 直 push 可
- トークン圧迫の兆候があれば（残 40% 下回り）`handoff/issue-31/YYYYMMDD_checkpoint.md` にスナップショット保存して新セッションへ
- 進捗は Issue #31 本文のチェックリストを `gh issue edit 31` でチェック更新

## 終了条件

各サブタスクの完了 = Issue #31 のチェックリスト 6 件すべてチェック。
ただし B1 / B2（forcing function 待ち）は対応保留でも Close 可（理由明記）。

## 開始手順

**まず以下を確認してから着手**:

- 不明点があれば質問（一度に 3 つまで）。プラン提示前に質問してクリアにする
- 質問がなければ「不明点なし」と明示してから状況把握レポート＋プラン提示へ
- どのサブタスクから始めるか提示（推奨は P0 #1 から）
- プラン承認後に作業開始

**質問例（参考）**:

- 「P0 (#1) から着手しますが、`consistency_check.md` 汚染修正と `claude_code_session.md` / `aar.md` 旧表記統一を 1 PR にまとめますか? 別 PR にしますか?」
- 「CLAUDE.md レビュー反映（#3）の Must 3 件は別 commit に分けますか? 1 commit でいいですか?」
- 「B1 / B2（#5, #6）は本エピックでは保留して Close 時の判断にする、で OK ですか?」
