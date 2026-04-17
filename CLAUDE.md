# ai-team-workflow リポジトリ 行動規範

See @ai-team/NOTE.md for detailed working rules.

このリポジトリは「AI チームワークフロー」のテンプレートとナレッジを育てるためのリポジトリ。
コードではなくドキュメントとプロンプトテンプレートが主な成果物。

## このリポジトリの構造

```text
docs/           ← ビジョン・プラン・ナレッジ（このリポジトリ自体の設計書）
ai-team/        ← 他リポジトリで使うテンプレート一式
  NOTE.md       ← 他リポジトリに持ち込む行動規範（チーム開発での摩擦を避けるため NOTE.md に統一）
  templates/    ← Gemini・Claude Code・AAR のプロンプトテンプレート
  handoff/      ← AI 間の成果物置き場（コミット対象）
    issue-XX/   ← Issue 番号でフォルダを分ける
```

## 作業時の基本ルール

- テンプレートを変更するときは `docs/knowledge.md` に変更理由と背景を記録する
- 他リポジトリでの実用フィードバックは `docs/knowledge.md` に追記する
- `docs/plan.md` の改善候補リストから着手するタスクを選ぶ
- 作業完了後は AAR を `ai-team/handoff/issue-XX/YYYYMMDD_aar.md` に保存する

## フィードバックループの回し方

```text
他リポジトリで使う
  → うまくいったこと・困ったことを docs/knowledge.md に記録
  → docs/plan.md の改善候補リストを更新
  → テンプレートや ai-team/NOTE.md を改善
  → commit & push
  → 次の利用時に改善版を使う
```

## NOTE.md について

`ai-team/NOTE.md` を他リポジトリで使う際の指示方法:

```text
「NOTE.md を読んでルールを把握して」
```

ルートに CLAUDE.md がある場合は以下を追記して参照させることもできる:

```text
# ルート CLAUDE.md に追記
See @ai-team/NOTE.md for detailed working rules.
```

チーム開発では CLAUDE.md をルートに置くと全員に影響するため、
NOTE.md という命名で摩擦を減らしている。

## セッション開始時

このリポジトリで作業するときは:

1. `docs/plan.md` の改善候補リストを確認する
2. `docs/knowledge.md` の未解決課題を把握する
3. 今日やることを決めてから作業開始
