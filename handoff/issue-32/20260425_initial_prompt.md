# Issue #32 新規セッション起動プロンプト

> **作成**: 2026-04-25
> **対応 Issue**: [#32](https://github.com/hirobirofran/ai-team-workflow/issues/32) [NOTE.md 拡張] ai-team/NOTE.md にセッション終了時手順テンプレを追加
> **状態**: **forcing function 待ち** — 他リポでの NOTE.md 実適用 FB を得てから着手

## 起動コマンド（コピペで使える）

### Claude Code 用（このファイルを Read させる）

```text
handoff/issue-32/20260425_initial_prompt.md を読んで、forcing function（他リポ FB）の所在を確認してから着手判断してください
```

---

## 起動条件（forcing function）

他リポジトリで `ai-team/NOTE.md` を 1 回以上実適用し、以下のいずれかが観察されたとき:

- 終了時手順が他リポでも有効と検証された → 持ち込みやすい形に整える
- 他リポでの実装パターンと現本リポ CLAUDE.md「セッション終了時の手順」（commit 17d9559 ）の差分が明確になった → テンプレ化スコープが固まる

forcing function が観察されていないなら**着手しない**。本セッションは「`handoff/followup.md` L247-273 を再確認して、最新状況を踏まえて再判断する」までで終了する。

## 着手前に必ず読むファイル

1. `CLAUDE.md`（自動ロード済）
2. 本 Issue [#32](https://github.com/hirobirofran/ai-team-workflow/issues/32) 本文
3. [`handoff/followup.md`](../followup.md) L247-273「B 取り組み案メモ」
4. [`handoff/issue-31/20260425_initial_prompt_review.md`](../issue-31/20260425_initial_prompt_review.md)（scope creep として #31 から切り出された経緯）
5. `ai-team/NOTE.md`（修正対象）
6. 本リポ `CLAUDE.md` の「セッション終了時の手順」セクション（テンプレ化対象、commit 17d9559 派生元）

## タスクの性質

- `ai-team/NOTE.md` への新規セクション追加（PR 必須、`ai-team/` 配下のため）
- **設計原則「テンプレファイル直編集禁則」**を踏まえ、代替設計を提案してから実装
  - 直接書き換えではなく: 本リポ CLAUDE.md 側からの参照で済ませる / 追記型ファイル別出し / 等
  - 具体策はユーザーと方針確認してから実装
- 議論駆動（実装前にユーザーとブレスト）

## 重要な制約

- `ai-team/` 配下は **PR 必須**（main 直 push 不可）
- 本リポは `handoff/followup.md` / `docs/knowledge.md` / `handoff/issue-XX/aar.md` の 3 層構造だが、他リポでは構成が違う可能性 → 翻訳設計が要る（personal-counselor の構成は未確認、必要なら別途調査）
- 「git push まで完了を絶対化」は他リポでも共通

## 終了条件

- `ai-team/NOTE.md` への追記が PR でマージされる
- Issue #32 を Close
- AAR を `handoff/issue-32/YYYYMMDD_aar.md` に保存

## 開始手順

1. forcing function の有無を確認（他リポでの NOTE.md 適用 FB が `handoff/followup.md` に記録されているか / ユーザーから言及があるか）
2. 不明点があれば質問（一度に 3 つまで）
3. 設計案（直編集禁則を回避する代替設計）を提示してユーザー確認
4. 承認後に実装 → PR 提出
