# business-flow-architect

会議の文字起こし(議事録 / Tactiq / TeamLog 等)から、**業務記述書 + drawio 業務フロー図 + 課題発生ポイント分析** の 3 点セットを 5 分で生成する Claude Code Skill。

商談ヒアリング / 社内の業務改善議論 / 経営会議 / 競合分析 / キックオフ など、文字起こしがある会議ならどれでも使えます。

---

## インストール (3 ステップ)

### 前提

- Claude Code がインストール済み([https://claude.ai/code](https://claude.ai/code))

### 手順

```bash
# 1. Claude Code を使うプロジェクトのルートに移動
cd path/to/your/project

# 2. .claude/skills/ 配下にクローン
git clone https://github.com/koji-hirayama-takea/business-flow-architect.git .claude/skills/business-flow-architect

# 3. Claude Code を起動して、こう投げる
#    → 「business-flow-architect をセットアップして」
```

セットアップは `CLAUDE.md` に従って自動進行します:

- `.mcp.json` に drawio MCP を自動追記
- 出力先ディレクトリを選んで作成(`outputs/` / プロジェクトルート / カスタム)
- 起動キーワードのカスタマイズ提案
- 動作確認

### 動作確認

セットアップ後、Claude Code で:

```
@hearings/サンプル.md の業務フロー一式を作って
```

または:

```
業務フロー作って
```

→ スキルが反応して、業務記述書 + drawio + 課題発生ポイント分析の 3 点セットが出力されればインストール完了。

---

## 使い方

Claude Code でこういう言葉を投げるだけで、スキルが起動します。

```
@meeting-2026-05-15.md の業務フロー一式を作って
```

```
先週の経営会議を drawio にして
```

```
今月のヒアリング 5 件をまとめて、共通する課題発生ポイントを抽出して
```

```
競合分析の議論を戦略図にして
```

スキルが自動でやること:

1. 文字起こしを読み込み、プロセスを時系列に抽出
2. **業務記述書** を Markdown で生成 → `./業務フロー/[プロセス名]_業務記述書_v1.0_YYYYMMDD.md`
3. **drawio XML** を生成 → `./業務フロー/[プロセス名]_業務フロー図_v1.0_YYYYMMDD.drawio`
4. drawio MCP で **ブラウザに自動プレビュー**
5. **課題発生ポイント分析** を独立 .md として保存
6. `P-XXX` / `F-XXX` / `S-XXX` / `C-XXX` が業務記述書と drawio で一致しているかチェック

体感: **5 分以内**で 3 ファイル + ブラウザプレビューまで完了。

---

## ファイル構成

```
business-flow-architect/
├── SKILL.md              ← スキル本体(起動キーワード + 実行手順)
├── README.md             ← 本ファイル
├── prompts/
│   └── base-prompt.md    ← マスタープロンプト(SKILL.md の元データ)
├── templates/
│   ├── 業務記述書テンプレート.md
│   ├── 業務フロー図テンプレート.drawio
│   └── チェックリストテンプレート.md
├── examples/
│   └── README.md         ← マスキング済み実例の説明
└── docs/
    ├── claude-code-setup.md  ← Claude Code 環境構築
    ├── customize.md           ← 起動キーワードのカスタマイズ
    └── troubleshooting.md     ← よくあるトラブル
```

---

## カスタマイズ

`SKILL.md` の `triggers` セクションに、自分の語彙でキーワードを追加できます。

```yaml
triggers:
  - 業務フロー作って       # デフォルト
  - 会議を drawio にして  # デフォルト
  - うちの会議を整理して   # カスタム追加
  - チームの仕事を図にして # カスタム追加
```

詳細は `docs/customize.md`。

---

## サポート

- 質問: 平山耕司 X([@hirayama_takea](https://x.com/hirayama_takea)) DM
- バグ報告: GitHub Issues(Collaborator 招待後)

---

## 改訂履歴

| 版 | 日付 | 内容 |
|---|---|---|
| 1.0 | 2026-05-27 | 初版。Claude Code Skill 化 + drawio MCP 統合 + 課題発生ポイント分析(`C-XXX`)追加 |
