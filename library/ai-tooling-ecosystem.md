# AI ツール群エコシステム

> Gemini CLI / Codex / Copilot など、AI コーディング支援ツールの「生物」情報スナップショット。
> 一次情報リンク必須。時系列降順（最新が上）。
> 詳細は [`library/index.md`](index.md) の運用ルールを参照。

---

### 2026-04-25: Gemini CLI と DeepResearch

- **情報源**:
  - [google-gemini/gemini-cli README](https://github.com/google-gemini/gemini-cli/blob/main/README.md)（最新 stable v0.39.1 / 2026-04-24）
  - [Gemini CLI Web tools tutorial](https://geminicli.com/docs/cli/tutorials/web-tools/)
  - [Issue #25165 Deep research integration into CLI](https://github.com/google-gemini/gemini-cli/issues/25165)（open / 2026-04-11 / status/need-triage）
  - [Gemini API: Deep Research Agent](https://ai.google.dev/gemini-api/docs/deep-research)（preview / Last updated 2026-04-21 UTC）
- **要点**:
  - **Gemini CLI 単体には DeepResearch 機能なし**。組込み Web ツールは `google_web_search` と `web_fetch` の 2 種のみ。Issue #25165 は要望段階（status/need-triage）
  - **Gemini API 側には Deep Research Agent が preview で存在**。Interactions API（`/v1beta/interactions`）経由、`background=True` 強制（20-60 分）、MCP サーバ連携対応。`generate_content` では呼べない
  - **CLI から DR を使う現実解**: ① CLI 外で API 直叩き、② MCP サーバ自作で CLI に統合、③ Web 版 Gemini Apps の DR を人間が叩く
- **このリポジトリへの影響**:
  - `ai-team/templates/researcher.md` の「DR プロンプトを生成して人間が Web 版 Gemini で実行」フローは **継続で OK**（CLI 自動化はまだ現実解になっていない）。テンプレ更新は **不要**
  - Issue #25165 / #23430 の進展で状況が変わったら本エントリを更新する
  - 詳細リサーチ記録: [`handoff/issue-28/20260425_research.md`](../handoff/issue-28/20260425_research.md)
