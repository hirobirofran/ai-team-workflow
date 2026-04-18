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
```

## 作業ルール

- **作業前にブランチを切る**: `git checkout -b feature/xxx` （main への直接コミット禁止）
- **コミット・プッシュ・PR は確認してから**: 内容を見せてからユーザーが承認後に実行
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

そのリポジトリの CLAUDE.md に参照を追記すると Claude Code がセッション開始時に自動で読む:

```text
See @NOTE.md for detailed working rules.
```

チーム開発では CLAUDE.md をルートに置くと全員に影響するため、NOTE.md という命名で摩擦を減らしている。

## セッション開始時

このリポジトリで作業するときは:

1. `docs/plan.md` の改善候補リストを確認する
2. `docs/knowledge.md` の未解決課題を把握する
3. 今日やることを決めてから作業開始
