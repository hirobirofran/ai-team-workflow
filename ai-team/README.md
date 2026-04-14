# AI チーム 運用ガイド

複数の AI を役割分担させて、コンテキスト爆発・暴走・ケアレスミスを減らすための運用手順書。

---

## 基本の考え方

### なぜ AI チームが必要か

| 問題 | 原因 | 対策 |
|------|------|------|
| 途中から AI が狂う | 会話が長くなりコンテキストが汚れる | 役割ごとにチャットを分ける |
| 推測で断定して暴走する | 整理と実装を同じ AI にやらせる | フェーズを分けて別 AI に渡す |
| ミスを指摘すると混乱する | 同じセッションで修正しようとする | 新規チャットで仕切り直す |
| 次回セッションで迷子になる | 引き継ぎがない | AAR を毎回 handoff/ に残す |

### AI の役割と得意不得意

| AI | 得意 | 苦手 | 担当フェーズ |
|----|------|------|------------|
| **Web Gemini 2.5 Pro** | 大量ドキュメント読込・Google Docs 連携・要件整理 | コーディング | PM・設計 |
| **Claude Code** | 総合力・ファイル操作・GitHub 連携・実装 | 超長期の文脈保持 | 設計確認・実装 |
| **Gemini CLI** | 巨大ファイルの一括読込・構造分析 | 実行・変更 | 補助（大ファイル分析のみ） |
| **GitHub Copilot** | IDE 内リアルタイム補完 | 要件理解 | 補助（補完のみ） |

---

## フロー全体図

```text
[人間のアイデア・背景資料]
        ↓
[Web Gemini / PM役]  ← Gem（推奨）または gemini_pm.md をコピペ
  要件を整理 → Gemini の出力を人間が handoff/spec.md にコピー
        ↓
[Web Gemini / 設計役]  ← Gem（推奨）または gemini_design.md をコピペ（別チャット！）
  タスク分解 → Gemini の出力を人間が handoff/task_spec.md にコピー
        ↓
[Claude Code]  ← templates/claude_code_session.md で起動
  GitHub Issue も読んで更に整理 → 確認 → 実装
  → 完了後 AAR を handoff/ に出力
        ↓
[次のセッション]
  Claude Code が handoff/ の AAR を読んで引き継ぎ
```

> **注意**: Web Gemini はファイルへの自動出力機能がない。
> Gemini の出力を人間が handoff/ にコピーするのが現状のワークフロー。
> 履歴を残したい場合は Google ドキュメントに貼り付けておくと後から参照できる。

---

## ステップ別手順

### Step 1: 要件整理（Web Gemini / PM役）

**Gem を使う場合（推奨）**: gemini_pm.md の内容でGemを作成しておく。Gemを開いて依頼内容を書くだけでよい。

**Gem がない場合**: 以下の手順でコピペ運用する。

1. Web Gemini を**新規チャット**で開く
2. [templates/gemini_pm.md](templates/gemini_pm.md) の `---ここから---` 〜 `---ここまで---` をコピーして貼り付ける
3. 続けて依頼内容を書く
4. Gemini が質問してきたら答える（推測で進めさせない）
5. 「整理が完了しました。以下でよいですか？」と聞いてきたら確認する
6. OKなら出力された内容を `handoff/spec.md` としてコピーして保存する
   （履歴用に Google ドキュメントにも貼っておくと後から参照できる）

> **ポイント**: 設計の話が始まったらそこで終了。設計は別チャットでやる。

---

### Step 2: タスク分解・設計（Web Gemini / 設計役）

**Gem を使う場合（推奨）**: gemini_design.md の内容でGemを作成しておく。Gemを開いて spec.md の内容を貼り付けるだけでよい。

**Gem がない場合**: 以下の手順でコピペ運用する。

1. Web Gemini を**新規チャット**で開く（Step 1 とは**別チャット**！）
2. [templates/gemini_design.md](templates/gemini_design.md) の `---ここから---` 〜 `---ここまで---` をコピーして貼り付ける
3. `handoff/spec.md` の内容をその下に貼り付ける
4. Gemini が確認してきたら承認する
5. 出力された内容を `handoff/task_spec.md` としてコピーして保存する

> **ポイント**: コードを書き始めたらそこで終了。実装は Claude Code に渡す。

---

### Step 3: 実装（Claude Code）

1. プロジェクトのリポジトリで Claude Code を起動する
2. [templates/claude_code_session.md](templates/claude_code_session.md) の `---ここから---` 〜 `---ここまで---` をコピーして貼り付ける
3. 必要であれば今日やるタスクを末尾に追記する
4. Claude Code が状況把握レポートを出してくるので確認する
5. 「この手順で進めてよいですか？」と聞いてきたら承認する
6. 実装が完了したら「AAR を templates/aar.md のフォーマットで handoff/ に保存して」と指示する

> **ポイント**: 最初の承認前にコードを書き始めたら止める。「まだ実装しないで」と言えば止まる。

---

### Step 4: セッション終了・引き継ぎ

Claude Code に以下を指示する：

```text
templates/aar.md のフォーマットで今日の作業の AAR を作成して
handoff/aar_YYYYMMDD_タスク名.md として保存して
```

次回セッション開始時に Claude Code が自動で読み込む。

---

## トラブル対応

### AI が推測で断定して進もうとする

```text
「推測で進まないで。不明な点を質問して。」
```

→ 同じチャットで言っても直らない場合は**新規チャットで仕切り直す**。

### AI がエラーで混乱し始めた

同じセッションで修正しようとしない。

```text
1. 現在のセッションを終了する
2. エラーの内容と「何をしたかったか」をメモする
3. 新規チャットを開き、メモを最初のメッセージに含める
```

### コンテキストが溢れそう（会話が長くなってきた）

```text
「今の状態を handoff/checkpoint_YYYYMMDD.md にまとめて」
→ 新規セッションで checkpoint を読ませて再開
```

### 大きなファイルを渡したい（Gemini CLI）

```bash
# ファイルが大きすぎて Claude Code に直接読ませたくない場合
gemini -p "@./path/to/largefile 構造と要点をmarkdownで整理して" > handoff/summary_largefile.md

# Claude Code には summary だけ読ませる
```

---

## ファイル構成

```text
ai-team/
  README.md                      ← この手順書
  CLAUDE.md                      ← Claude Code の行動規範（リポジトリルートにコピーして使う）
  templates/
    gemini_pm.md                 ← Step 1 で使う
    gemini_design.md             ← Step 2 で使う
    claude_code_session.md       ← Step 3 で使う
    aar.md                       ← Step 4 の出力フォーマット
  handoff/                       ← AI 間の成果物置き場
    spec.md                      ← Step 1 の出力
    task_spec.md                 ← Step 2 の出力
    aar_YYYYMMDD_タスク名.md     ← Step 4 の出力
```

---

## Claude Code セットアップ（ライセンス取得後）

1. `ai-team/CLAUDE.md` をプロジェクトのリポジトリルートにコピーする
2. `ai-team/handoff/` ディレクトリをプロジェクトに作る（`.gitignore` に追加するか否かはプロジェクトによる）
3. Claude Code を起動して `templates/claude_code_session.md` を貼り付ける

### CLAUDE.md の読ませ方（サブフォルダに置く場合）

`ai-team/CLAUDE.md` はリポジトリルートにないため、以下いずれかの方法で読ませる。

```text
方法1（推奨）: ~/.claude/CLAUDE.md にコピーする
  → 全プロジェクト共通の個人設定になる。リポジトリに入らない。
  → チームに影響しないので合意不要。

方法2: リポジトリルートの CLAUDE.md から参照する
  # CLAUDE.md（ルート）に1行追加
  See @ai-team/CLAUDE.md for working rules

方法3: セッション開始時に明示的に読ませる
  「ai-team/CLAUDE.md を読んでルールを把握して」
```

---

## チーム開発での運用

### このフォルダはそのままコミットして問題ない

`ai-team/` はサブフォルダなので既存ファイルとは衝突しない。
ただし以下の点に注意する。

### handoff/ は .gitignore に追加する

```gitignore
# .gitignore に追加
ai-team/handoff/*
!ai-team/handoff/.gitkeep
```

handoff/ には作業中の中間ファイルが大量に生まれる。
ソースコードではないのでコミット不要。
履歴を残したい場合は個人ブランチで管理する。

```bash
# 個人ブランチで AAR だけ保存する場合
git checkout -b ai-worklog/yourname
git add ai-team/handoff/aar_*.md
git commit -m "ai worklog: TASK-003 完了"
```

### フォルダ名について

チーム開発では最初から `ai-team/` と置くと驚かれることがある。
個人で試している段階は `ai-team-yourname/` にしておき、
チームへの展開が決まった段階でリネームするのが摩擦が少ない。

### CLAUDE.md のチーム展開ロードマップ

```text
Phase 1（個人試用）:
  ~/.claude/CLAUDE.md に個人設定 → チームに影響なし

Phase 2（効果確認後、チームに提案）:
  ai-team/CLAUDE.md をそのままコミット → 任意参加

Phase 3（チーム標準化）:
  リポジトリルートの CLAUDE.md に昇格 → 全員に適用
```

### handoff/ の内容はソースコードではない

```text
handoff/ に置くもの（AI間の連絡ファイル）:
  spec.md, task_spec.md, aar_*.md, checkpoint_*.md

handoff/ に置かないもの:
  実装したソースコード → 通常のソースツリーへ
  テストコード → 通常のソースツリーへ
```

---

## 今すぐ使えること（Claude Code なしでも）

- **Step 1・2**: Web Gemini（または Gem）だけで今日から使える
- **Step 3**: Claude.ai Projects（有料）またはそのまま Web 版 Claude でも代用可
- **Step 4**: どの AI でも「AAR を書いて」と指示すれば動く
