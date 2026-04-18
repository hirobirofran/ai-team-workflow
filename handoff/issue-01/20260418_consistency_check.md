# 整合性チェック結果

**実行日**: 2026-04-18
**対象**: ai-team-workflow リポジトリ（feature/structure-and-consistency-check ブランチ）
**実行者**: Claude Code

---

## 🔴 問題あり（要対応）

| # | 種別 | 場所 | 内容 |
|---|------|------|------|
| 1 | 未記載ファイル | `ai-team/handoff/review/` | ディレクトリが存在するが、どの説明にも記載なし（ファイルを移動したが空ディレクトリが残存） |
| 2 | ファイル間矛盾 | `ai-team/README.md` 構成図 vs 本文 | 構成図では `handoff/` を「テンプレート用空ディレクトリ」と書いているが、本文では `handoff/issue-XX/YYYYMMDD_*.md` にファイルを置く前提の記述が多数ある |
| 3 | 未記載ファイル | `README.md`（ルート） | 新設の `handoff/` が構造図に記載なし |
| 4 | リンク不整合 | `CLAUDE.md` L54 | `See @NOTE.md` — NOTE.md はこのリポジトリのルートに存在しない（`ai-team/NOTE.md`）。コピー先での記法の説明が不明確 |

## 🟡 軽微（できれば対応）

| # | 種別 | 場所 | 内容 |
|---|------|------|------|
| 1 | docs乖離 | `docs/plan.md` | 完了済み ✅ になっていない項目が実際には完了している（researcher.md, 工数見積もり等） |

## 🟢 問題なし

- テンプレートファイル全種が `ai-team/README.md` の構成図に記載されている
- `CLAUDE.md` の作業ルールと実際の運用方針に矛盾なし
- `handoff/issue-01/` 構造は `CLAUDE.md` の記述と一致

---

## 修正内容（本チェック後に対応）

### 修正済み（このコミットで対応）

- [x] ai-team/handoff/review/ 空ディレクトリの削除
- [x] ai-team/README.md 構成図に handoff 構造を復元
- [x] ルート README.md に handoff/ を追記
- [x] CLAUDE.md の `See @NOTE.md` を明確化（コピー先での使い方と注記追加）

### 別途対応

- [ ] docs/plan.md の完了チェックリストを更新
