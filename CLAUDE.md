# ai-team-workflow リポジトリ 行動規範

このリポジトリは「AI チームワークフロー」のテンプレートとナレッジを育てるためのリポジトリ。
コードではなくドキュメントとプロンプトテンプレートが主な成果物。

> **注意**: `ai-team/NOTE.md` はこのリポジトリで使うものではなく、**他リポジトリに持ち込む**行動規範です。
> このリポジトリ自体は以下のルールに従って運用します。

## このリポジトリの構造

```text
docs/           ← ビジョン・プラン・ナレッジ（このリポジトリ自体の設計書）
ai-team/        ← 他リポジトリで使うテンプレート一式
  NOTE.md       ← 他リポジトリに持ち込む行動規範（このリポジトリでは使わない）
  templates/    ← Gemini・Claude Code・AAR のプロンプトテンプレート
  handoff/      ← テンプレート用空ディレクトリ（他リポジトリへのコピー見本）
handoff/        ← このリポジトリ自身の作業ファイル置き場
  issue-XX/     ← Issue 番号でフォルダを分ける
  misc/         ← Issue に紐づかない作業・サンプル
library/        ← このリポジトリ固有の一次情報スナップショット（AI ツール・モデル・MCP）
```

## 作業ルール

- **作業前にブランチを切る**（原則）: `git checkout -b feature/xxx`
- **PR が必須なのは `ai-team/` 配下の変更のみ**（テンプレなのでレビューしたい）
- **`docs/` `handoff/` `library/` 配下は main への直接 push 可**（このリポジトリ固有の作業ファイルなので、軽量・低摩擦で進める）
- **コミット・プッシュ・PR は確認してから**: 内容を見せてからユーザーが承認後に実行（PR 経由でなくても同じ）
- **整理は append-only で溜めずに、編集時に consolidate**：knowledge.md / followup.md 等に追記するときは、同じトピックの既存エントリが3件以上あるなら原則抽出を検討する（autovacuum 相当）
- **議論で合意した運用方針はその場で `handoff/followup.md` に書く**: コンテキストウィンドウ依存禁止。手動コンパクト・次セッション開始でも生き残る場所に永続化する（2026-04-25 反省より）
- **マージ後はブランチを削除する**: リモート・ローカル両方（`git push origin --delete <branch>` → `git branch -d <branch>`）
- **変更後・PR 前に整合性チェックを実行**: `ai-team/templates/consistency_check.md` を使う
- テンプレートを変更するときは `docs/knowledge.md` に変更理由と背景を記録する
- 他リポジトリでの実用フィードバックは `docs/knowledge.md` に追記する
- `docs/plan.md` の改善候補リストから着手するタスクを選ぶ
- 作業完了後は AAR を `handoff/issue-XX/YYYYMMDD_aar.md` に保存する
- セッション開始時のプランは `handoff/issue-XX/YYYYMMDD_plan.md` に保存する

## フィードバックループの回し方

```text
他リポジトリで使う
  → うまくいったこと・困ったことを docs/knowledge.md に記録
  → docs/plan.md の改善候補リストを更新
  → テンプレートや ai-team/NOTE.md を改善
  → commit & push
  → 次の利用時に改善版を使う
```

## NOTE.md の使い方（他リポジトリ向け）

`ai-team/NOTE.md` を**他のリポジトリ**に持ち込んで使う。ファイル名は任意。

```bash
cp ai-team/NOTE.md /path/to/your-project/NOTE.md
```

コピー先のリポジトリの CLAUDE.md に参照を追記すると Claude Code がセッション開始時に自動で読む:

```text
See @ai-team/NOTE.md for detailed working rules.
```

チーム開発では CLAUDE.md をルートに置くと全員に影響するため、NOTE.md という命名で摩擦を減らしている。

## セッション開始時

ユーザーから挨拶や作業開始の合図があったら、**まず起動契機を判別** する。

### A. `initial_prompt.md` 経由の起動

「`handoff/issue-XX/YYYYMMDD_initial_prompt.md` を読んで」と指示された場合は、**そのファイルの指示に従う**（独自 Step が定義されている）。以下の Step 1〜4 は実行しない。

### B. 通常起動（挨拶・「続きやる」等）

以下の順で動く。

1. **既存 Issue を確認**: `gh issue list --limit 30` で現状把握。前回までに何が溜まっているか、何が解決済みか
2. **永続化されたコンテキストを読む**:
   - `docs/plan.md` の改善候補リスト
   - `docs/knowledge.md` の未解決課題（特に最新セクション）
   - `handoff/followup.md` の次回候補・申し送り
3. **現状を一言で要約してユーザーに見せる**: 「Open Issue が N 件、前回は○○の話でした。followup.md に△△の候補があります。今日は何から?」のように
4. **ユーザーが選ぶ**: 「続きやる」「新しい話」「今日はちょっとだけ」等。**ユーザーの選択を待ってから着手**（こちらから先回りして提案しない）

## セッション終了時の手順

ユーザーから「今日はここまで」「終わる」等が来たら、以下を完了させてから閉じる。

1. **このセッションで合意した運用方針・気づきは永続化**:
   - 軽量メモ・次回候補: `handoff/followup.md`
   - 長期ナレッジ・原則: `docs/knowledge.md`（同トピック 3 件以上で consolidate 検討）
   - Issue 単位の振り返り: `handoff/issue-XX/YYYYMMDD_aar.md`
   - memory（`~/.claude/projects/.../memory/`）の状態が古ければ更新
2. **`git add` → `git commit` → `git push origin main`** を完了させる（push 漏れはコンテキストウィンドウ依存になるので絶対）
3. 新規セッションへパスする作業がある場合、`handoff/issue-XX/YYYYMMDD_initial_prompt.md` が整備されているか確認
4. 「お疲れさまでした」で閉じる。粘らない

## 運用改善サイクル

CLAUDE.md / handoff/ / templates/ など、運用自身にも気づきは出る。改善は以下の閾値で動かす（personal-counselor の自発進化メカニズムを意識的に再現するため）。

1. **気づきを拾う**: セッション中に運用ルール・手順・ファイル構成・テンプレへの気づきがあったら、セッション末（または気づいたその場）で永続化する
   - 短期メモ・次回候補: `handoff/followup.md`
   - 長期ナレッジ・原則: `docs/knowledge.md`
2. **小さな改善はその場で**: 文言修正・節追加・既存セクション強化・典型的な手順化は、CLAUDE.md / docs / handoff/ をそのまま直して commit & push（main 直 push 可な領域）
3. **大きな変更は Issue 化**: 以下は **Issue を起票してユーザーに可否を聞く**（勝手に実行しない）
   - 構造変更（フォルダ構成・ファイル分割・命名規約変更）
   - 新ファイル・新ディレクトリ追加
   - 方針転換（運用ルールの根本変更・既存ルールの撤廃）
   - `ai-team/` 配下の変更（テンプレなので必ず PR レビュー）

「小さい / 大きい」の判断に迷ったら **大きい側** に倒す（Issue は問題提起だけにとどめ、具体策に踏み込みすぎない）。
