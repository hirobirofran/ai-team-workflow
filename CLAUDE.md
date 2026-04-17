# ai-team-workflow リポジトリ 行動規範

このリポジトリは「AI チームワークフロー」のテンプレートとナレッジを育てるためのリポジトリ。
コードではなくドキュメントとプロンプトテンプレートが主な成果物。

## このリポジトリの構造

```text
docs/           ← ビジョン・プラン・ナレッジ（このリポジトリ自体の設計書）
ai-team/        ← 他リポジトリで使うテンプレート一式
  CLAUDE.md     ← 他リポジトリに持ち込む Claude Code 行動規範
  templates/    ← Gemini・Claude Code・AAR のプロンプトテンプレート
  handoff/      ← AI 間の成果物置き場（コミット対象）
```

## 作業時の基本ルール

- テンプレートを変更するときは `docs/knowledge.md` に変更理由と背景を記録する
- 他リポジトリでの実用フィードバックは `docs/knowledge.md` に追記する
- `docs/plan.md` の改善候補リストから着手するタスクを選ぶ
- 作業完了後は AAR を `ai-team/handoff/` に保存する

## フィードバックループの回し方

```text
他リポジトリで使う
  → うまくいったこと・困ったことを docs/knowledge.md に記録
  → docs/plan.md の改善候補リストを更新
  → テンプレートや ai-team/CLAUDE.md を改善
  → commit & push
  → 次の利用時に改善版を使う
```

## ファイル命名について

`ai-team/CLAUDE.md` は Claude Code が自動で読む特別なファイル名。
チーム開発リポジトリで使いにくい場合の代替命名:

- `NOTE.md` → セッション開始時に「NOTE.md を読んでルールを把握して」と指示
- `AI_GUIDE.md` など任意の名前でも同様に使える

## セッション開始時

このリポジトリで作業するときは:

1. `docs/plan.md` の改善候補リストを確認する
2. `docs/knowledge.md` の未解決課題を把握する
3. 今日やることを決めてから作業開始
