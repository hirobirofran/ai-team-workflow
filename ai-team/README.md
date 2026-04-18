# AI チーム 運用ガイド

複数の AI を役割分担させて、コンテキスト爆発・暴走・ケアレスミスを減らすための運用手順書。

---

## 基本の考え方

### なぜ AI チームが必要か

| 問題 | 原因 | 対策 |
| --- | --- | --- |
| 途中から AI が狂う | 会話が長くなりコンテキストが汚れる | 役割ごとにチャットを分ける |
| 推測で断定して暴走する | 整理と実装を同じ AI にやらせる | フェーズを分けて別 AI に渡す |
| ミスを指摘すると混乱する | 同じセッションで修正しようとする | 新規チャットで仕切り直す |
| 次回セッションで迷子になる | 引き継ぎがない | AAR・プランを handoff/ に残す |

### AI の役割と得意不得意

| AI | 得意 | 苦手 | 担当フェーズ |
| --- | --- | --- | --- |
| **NotebookLM** | 大量ドキュメントの Deep Research | 要件整理・コーディング | リサーチ |
| **Web Gemini 2.5 Pro** | 大量ドキュメント読込・要件整理 | コーディング | PM・設計 |
| **Claude Code** | 総合力・ファイル操作・GitHub 連携・実装 | 超長期の文脈保持 | 設計確認・実装 |
| **Gemini CLI** | 巨大ファイルの一括読込・構造分析 | 実行・変更 | 補助（大ファイル分析のみ） |
| **GitHub Copilot** | IDE 内リアルタイム補完 | 要件理解 | 補助（補完・Claude Code 上限時の代替） |

---

## フロー全体図

```text
[人間のアイデア・背景資料]
        ↓
[NotebookLM / リサーチャー役]  ← templates/researcher.md を使う
  DeepResearch で事前調査 → 人間が handoff/issue-XX/YYYYMMDD_research.md にコピー
        ↓
[Web Gemini / PM役]  ← Gem（推奨）または gemini_pm.md をコピペ
  要件を整理 → 人間が handoff/issue-XX/YYYYMMDD_spec.md にコピー
        ↓
[Web Gemini / 設計役]  ← Gem（推奨）または gemini_design.md をコピペ（別チャット！）
  タスク分解 → 人間が handoff/issue-XX/YYYYMMDD_task_spec.md にコピー
        ↓
[Claude Code]  ← templates/claude_code_session.md で起動
  「NOTE.md を読んでルールを把握して」から開始
  GitHub Issue も読んで更に整理 → 確認 → 実装
  → プランを handoff/issue-XX/YYYYMMDD_plan.md に保存
  → 完了後 AAR を handoff/issue-XX/YYYYMMDD_aar.md に出力
        ↓
[次のセッション]
  直近の plan.md と aar.md を読んで引き継ぎ
```

> **注意**: Web Gemini はファイルへの自動出力機能がない。
> Gemini の出力を人間が handoff/ にコピーするのが現状のワークフロー。
> 履歴を残したい場合は Google ドキュメントにも貼っておくと参照しやすい。

---

## ファイル構成

```text
ai-team/
  NOTE.md                      ← Claude Code 行動規範（他リポジトリに持ち込む）
  README.md                    ← この手順書
  templates/
    researcher.md              ← Step 0: リサーチャー役（NotebookLM 連携）
    gemini_pm.md               ← Step 1: PM役
    gemini_design.md           ← Step 2: 設計役
    claude_code_session.md     ← Step 3: 実装セッション開始
    pr_review.md               ← Step 3.5: PR レビュー（タスク仕様との照合）
    consistency_check.md       ← 随時: ドキュメント整合性チェック（未記載・矛盾・乖離）
    checkpoint.md              ← 随時: コンテキスト引き継ぎフォーマット
    aar.md                     ← Step 4: AAR フォーマット
  handoff/                     ← 他リポジトリにコピーして使う作業ファイル置き場（見本）
    issue-XX/                  ← Issue 番号でフォルダを切る
      YYYYMMDD_research.md
      YYYYMMDD_spec.md
      YYYYMMDD_task_spec.md
      YYYYMMDD_plan.md         ← 最重要：Copilot 引き継ぎにも使う
      YYYYMMDD_aar.md
      YYYYMMDD_checkpoint.md   ← コンテキスト溢れ時の引き継ぎ
    misc/                      ← Issue に紐づかない作業
```

---

## ステップ別手順

### Step 0: リサーチ（オプション）

要件が複雑・技術的に不確かな場合に実施。どちらのツールを使うかは状況次第。

**ツール選択**:

| ツール | 向いているケース |
| --- | --- |
| **NotebookLM** | 手元に PDF・仕様書・URL がある。ソース追加時に「DeepResearch」オプションを選ぶと検索も統合できる |
| **DeepResearch（Gemini）** | GitHub リポジトリを添付したい。最新の技術動向を広く調べたい |

**手順**:

1. 上記いずれかで調査する
2. 結果が長い場合は Gemini に「要点を整理して」と依頼してから次へ
3. 調査結果をテキストで貼り付けて [templates/researcher.md](templates/researcher.md) のフォーマットに整理する
4. 出力を `handoff/issue-XX/YYYYMMDD_research.md` に保存する
5. Step 1 で Gemini PM役に渡す

> **ポイント**: Gemini にファイル添付しても読まないことがある。テキストで直接貼り付ける。

---

### Step 1: 要件整理（Web Gemini / PM役）

**Gem を使う場合（推奨）**: gemini_pm.md の内容で Gem を作成しておく。Gem を開いて依頼内容を書くだけでよい。

**Gem がない場合**:

1. Web Gemini を**新規チャット**で開く
2. [templates/gemini_pm.md](templates/gemini_pm.md) の `---ここから---` 〜 `---ここまで---` をコピーして貼り付ける
3. research.md の内容と依頼内容を続けて書く
4. Gemini が質問してきたら答える（推測で進めさせない）
5. 承認後、出力を `handoff/issue-XX/YYYYMMDD_spec.md` にコピーして保存する

> **ポイント**: 設計の話が始まったらそこで終了。設計は別チャットでやる。

---

### Step 2: タスク分解・設計（Web Gemini / 設計役）

**Gem を使う場合（推奨）**: gemini_design.md の内容で Gem を作成しておく。

**Gem がない場合**:

1. Web Gemini を**新規チャット**で開く（Step 1 とは**別チャット**！）
2. [templates/gemini_design.md](templates/gemini_design.md) の `---ここから---` 〜 `---ここまで---` をコピーして貼り付ける
3. `handoff/issue-XX/YYYYMMDD_spec.md` の内容を貼り付ける
4. 承認後、出力を `handoff/issue-XX/YYYYMMDD_task_spec.md` にコピーして保存する

> **ポイント**: コードを書き始めたらそこで終了。実装は Claude Code に渡す。

---

### Step 3: 実装（Claude Code）

1. プロジェクトのリポジトリで Claude Code を起動する
2. 最初に「NOTE.md を読んでルールを把握して」と指示する
3. [templates/claude_code_session.md](templates/claude_code_session.md) の `---ここから---` 〜 `---ここまで---` を貼り付ける
4. 今日やるタスクを末尾に追記する
5. Claude Code が状況把握レポートと工数見積もりを出してくるので確認する
6. 「この手順で進めてよいですか？」の確認後、承認する
7. 承認後、Claude Code がプランを `handoff/issue-XX/YYYYMMDD_plan.md` に自動保存する

> **ポイント**: プランファイルは Claude Code のレート上限時に Copilot への引き継ぎにも使う。

---

### Step 3.5: PR レビュー（オプション）

実装完了後、PR 作成前後に実施する。Copilot PR レビューより複数ファイルの整合性確認が得意。

```text
templates/pr_review.md の ---ここから--- 以下を貼り付けて、PR番号を伝える
```

> **使い分け**: Copilot PR レビュー → コード品質・スタイル。Claude Code → タスク仕様との照合・抜け漏れ。

---

### Step 4: セッション終了・引き継ぎ

Claude Code に以下を指示する：

```text
handoff/issue-XX/YYYYMMDD_aar.md として AAR を保存して
（templates/aar.md のフォーマットを使って）
```

次回セッション開始時に直近の `plan.md` と `aar.md` を読んで引き継ぎ。

---

## Claude Code のレート上限に達したとき

Claude Code の 5 時間枠が切れた場合の対応:

1. `handoff/issue-XX/YYYYMMDD_plan.md` を GitHub Copilot に見せる
2. 「このプランの続きをやってほしい。どこまで完了しているかは AAR を見て」と指示する
3. Copilot は要件理解が弱いので、プランの該当タスクを明示的に指定する

---

## トラブル対応

### AI が推測で断定して進もうとする

```text
「推測で進まないで。不明な点を質問して。」
```

同じチャットで言っても直らない場合は**新規チャットで仕切り直す**。

### AI がエラーで混乱し始めた

同じセッションで修正しようとしない。

```text
1. 現在のセッションを終了する
2. エラーの内容と「何をしたかったか」をメモする
3. 新規チャットを開き、メモを最初のメッセージに含める
```

### コンテキストが溢れそう（会話が長くなってきた）

```text
「今の状態を handoff/issue-XX/YYYYMMDD_checkpoint.md にまとめて」
→ 新規セッションで checkpoint を読ませて再開
```

---

## NOTE.md の使い方（他リポジトリへの持ち込み）

`ai-team/NOTE.md` をコピーして使う。ファイル名は任意。

```bash
cp ai-team/NOTE.md /path/to/your-project/NOTE.md
```

セッション開始時の指示:

```text
「NOTE.md を読んでルールを把握して」
```

チーム開発でリポジトリルートの `CLAUDE.md` に影響を出したくない場合は
`NOTE.md` という名前のままにしておくと目立たない。

---

## 今すぐ使えること（Claude Code なしでも）

- **Step 0〜2**: NotebookLM + Web Gemini だけで今日から使える
- **Step 3**: Claude.ai Projects（有料）またはそのまま Web 版 Claude でも代用可
- **Step 4**: どの AI でも「AAR を書いて」と指示すれば動く
