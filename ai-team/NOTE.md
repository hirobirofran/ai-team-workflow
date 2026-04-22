# AI チーム 行動規範（NOTE.md）

このファイルは `NOTE.md` という名前で他リポジトリに持ち込んで使う。
セッション開始時に「NOTE.md を読んでルールを把握して」と指示する。

## 基本方針

- **整理が先、実装は後**：セッション開始時は必ず状況把握・整理を行い、ユーザーの承認後に実装を開始する
- **推測での断定禁止**：不明な点は推測で進めず、必ず質問して確認する
- **スコープ厳守**：指示されたタスク以外のファイルを変更しない。要件にない機能を追加しない
- **こまめな状況報告**：作業の区切りごとに進捗・残タスク・懸念点を報告する

## セッション開始手順

1. リポジトリ構造を把握する（ファイルツリー確認）
2. `handoff/` ディレクトリに成果物があれば読む（特に直近の AAR とプラン）
3. 関連する GitHub Issue を確認する（`gh issue list` → 該当イシューを `gh issue view`）
4. `task_spec.md` があれば読む
5. コミット規約・ブランチ規約を確認する（不明な場合はユーザーに質問する）
6. 作業ブランチを作成する（`git checkout -b feature/xxx` など）
7. 作業内容・影響ファイル・手順・**工数見積もり**を整理してユーザーに提示する
8. **「これで進めてよいですか？」と確認してから実装開始**
9. 承認後、作業計画を `handoff/issue-XX/YYYYMMDD_plan.md` に保存する

## 工数見積もりルール

- セッション開始時の整理レポートに必ず工数見積もりを含める
- 実装中は区切りごとに残工数を更新して報告する
- 見積もりが外れそうなときは早めに報告する

## ブランチ・コミットルール

- 作業開始前に必ずブランチを切る（main/master への直接コミット禁止）
- ブランチ名の規約はプロジェクトの規約に従う（不明な場合は質問する）
- コミットメッセージの規約はプロジェクトの規約に従う（不明な場合は質問する）

## クリティカル操作は必ず確認

以下の操作は必ずユーザーに確認してから実行する:

- `git commit`（コミット内容を見せてから）
- `git push` / PR 作成
- ファイル・ディレクトリの削除
  - `rm -rf`（特にワイルドカード含む場合）
- 既存データの上書き
- **インフラ操作**
  - `terraform apply` / `terragrunt apply` / `terragrunt run-all apply`
  - `cdk deploy`
  - `kubectl apply` / `kubectl delete`
  - `aws` CLI の書き込み系（`s3 rm`, `iam delete-*`, `ec2 terminate-instances` 等）
  - `gcloud` / `az` CLI の書き込み系
- **データベース直接操作**
  - `DROP` / `TRUNCATE` / `DELETE without WHERE`
  - マイグレーション未検証の `migrate`
- 外部サービスへの書き込みリクエスト

## 仕様制約・別 Issue 候補の発見時

実装中に「別 Issue 化すべき」と判断した事項は、作業を止めず、
以下のコマンドで下書き URL を生成して報告する:

```bash
gh issue create --title "..." --body "..." --web
```

`--web` オプションで Issue 作成画面がブラウザで開かれるのみで、実際には投稿されない。
ユーザーは内容を確認して送信するだけでよい。
Claude Code 側はコマンド実行後、URL と一行要約を報告して作業を継続する。

**使い分け**:

- 実装中に発見 → `gh issue create --web` で即下書き
- セッション終了時にまとめて → AAR の「発見した仕様・制約」欄

## ファイルサイズルール

- 単一ファイルが大きい場合（目安: 500行超）は先に Gemini CLI でサマリを生成してから処理する

  ```bash
  gemini -p "@./path/to/largefile 構造と要点をmarkdownで整理して" > handoff/misc/YYYYMMDD_summary_largefile.md
  ```

- ディレクトリ丸ごと分析が必要な場合も同様

  ```bash
  gemini -p "@./path/to/dir/ このディレクトリの全ファイルの構造と依存関係を整理して" > handoff/misc/YYYYMMDD_summary_dir.md
  ```

## サブエージェント使用ルール

- タスクを並列実行できる場合は Agent ツールでサブエージェントを活用する
- 各サブエージェントには **task_spec.md の該当タスクのみ** を渡す（全体コンテキストを渡さない）
- サブエージェントへの禁止事項を明示する（スコープ外変更禁止など）

## ハンドオフ管理

`handoff/` ディレクトリは **AI 間の連絡ファイル専用**。実装したソースコードは置かない。

### フォルダ構成

```text
handoff/
  issue-XX/               ← 対応 Issue 番号でフォルダを切る
    YYYYMMDD_research.md  ← リサーチ結果
    YYYYMMDD_spec.md      ← 要件仕様書
    YYYYMMDD_task_spec.md ← タスク仕様書
    YYYYMMDD_plan.md      ← Claude Code 初期プラン（最重要：Copilot 引き継ぎにも使う）
    YYYYMMDD_aar.md       ← After Action Review
    YYYYMMDD_checkpoint.md ← コンテキスト引き継ぎ（コンテキスト溢れ時）
```

- ファイル名は必ず日付プレフィックス（`YYYYMMDD_`）をつける
- Issue に紐づかない作業は `handoff/misc/` に置く
- セッション開始時の作業計画は必ず `handoff/issue-XX/YYYYMMDD_plan.md` に保存する
- 実装完了後は必ず `handoff/issue-XX/YYYYMMDD_aar.md` に AAR を記録する

### チェックポイント保存ルール

- 作業時間が 2h を超えたら、タスクの区切りごとに checkpoint を保存する
- コンテキストが溢れそうなときは即座に保存する（フォーマット: `templates/checkpoint.md`）
- 保存の指示例：「今の状態を `handoff/issue-XX/YYYYMMDD_checkpoint.md` に保存して」

## セーフティネット

指示と異なることをしようとしている場合は、実行前に報告する:

```text
「⚠️ 指示の範囲を超える可能性があります: [何をしようとしているか]
   続けてよいですか？」
```

## エラー発生時

- エラーが出ても慌てない。原因を分析してから対処する
- 同じアプローチを繰り返さない。別の方法を検討する
- 手に負えない場合は「詰まっています。状況を整理します」とユーザーに報告する
