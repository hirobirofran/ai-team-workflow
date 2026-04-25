# プラン：今回セッションの知見を CLAUDE.md・knowledge.md にフィードバック

## Context

Issue #1 の PR サイクル（ブランチ作成→実装→PR→レビュー→修正→マージ→ブランチ削除）を通じて、
このリポジトリの CLAUDE.md に不足しているルールが明確になった。
あわせて CLAUDE.md のパフォーマンス（行数・トークン数）に関する知見を docs/knowledge.md に記録する。

---

## TASK-1: CLAUDE.md に3つのルールを追記

**変更対象**: `CLAUDE.md` の「作業ルール」セクション

追加する内容:

```markdown
- **PR 作成時はレビュー依頼を忘れずに**: `--reviewer hirobirofran`
- **マージ後はブランチを削除する**: リモート・ローカル両方
  - `git push origin --delete <branch>` → `git branch -d <branch>`
- **変更後・PR 前に整合性チェックを実行する**: `templates/consistency_check.md` を使う
```

現在 65行 → 追加後 70行前後。パフォーマンス上問題なし。

---

## TASK-2: docs/knowledge.md に CLAUDE.md パフォーマンスの知見を追記

**変更対象**: `docs/knowledge.md`

追加する内容（新セクション「CLAUDE.md の設計」）:

- **行数目安**: 非公式だが 100行前後からコンテキスト消費が増えトークン効率が落ちると言われている
- **チェック方法**: `wc -l CLAUDE.md`（行数）、`wc -w CLAUDE.md`（単語数）で定期確認
- **日本語のトークン換算**: 日本語は1文字 ≈ 1〜2トークン。英語より膨張しやすい
- **対策（軽い順）**:
  1. 詳細ルールは `@別ファイル` で参照し CLAUDE.md 本体を薄く保つ（このリポジトリの NOTE.md 分離が好例）
  2. コードブロックを減らす（トークンを食いやすい）
  3. **溢れたらスキルに移動**: Claude Code のカスタムスラッシュコマンド（`.claude/commands/` 以下に md ファイルを置く）として切り出すと、必要なときだけロードできる。常時読み込みの CLAUDE.md より効率的。
- **スキルの使い方**:
  - `.claude/commands/review.md` → `/review` コマンドで呼び出し
  - `~/.claude/commands/` に置くと全プロジェクト共通で使える
  - CLAUDE.md に「〇〇は `/review` を使う」と一行書くだけでよい
- **現状**: このリポジトリの CLAUDE.md は 65行。NOTE.md は 103行。合計でも許容範囲内。

---

## 実装順序

0. このプランを `handoff/misc/20260418_plan_claude_feedback.md` に保存
1. ブランチ作成: `git checkout -b chore/feedback-to-claude-md`
2. TASK-1: CLAUDE.md 追記
3. TASK-2: docs/knowledge.md 追記
4. コミット・プッシュ・PR 作成（--reviewer hirobirofran）

---

## 検証

- CLAUDE.md の行数が 100行未満に収まっていること
- 追加したルールが実際の運用と矛盾しないこと
