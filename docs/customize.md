# Skill カスタマイズ手順

`SKILL.md` の `triggers` セクションに、自分の語彙でキーワードを追加すれば、Claude Code が反応するようになります。

## 起動キーワードを追加する

`SKILL.md` の冒頭フロントマター(`---` で囲まれた部分)を編集します。

### Before(デフォルト)

```yaml
---
name: business-flow-architect
description: 会議の文字起こしから...
triggers:
  - 業務フロー作って
  - 会議を drawio にして
  - ヒアリングを構造化して
  - 課題発生ポイント抽出
  - 業務記述書を作って
  ...
---
```

### After(カスタム追加)

```yaml
---
name: business-flow-architect
description: 会議の文字起こしから...
triggers:
  - 業務フロー作って
  - 会議を drawio にして
  # ↓ ここから自分のカスタム
  - うちの会議を整理して
  - チームの仕事を図にして
  - 商談メモを構造化
  - PdM 起案のフロー作って
  - QBR 議論を drawio に
---
```

## 部署用語をスキル本体に反映する

たとえば「**営業企画部**」が頻出するチームなら、`SKILL.md` の本文側で次のように上書きできます。

```markdown
## このスキルの使い方 (社内最適化版)

ユーザーが起動したら、スイムレーンを以下の組織構造で初期化する:

- 営業企画部
- インサイドセールス
- フィールドセールス
- カスタマーサクセス
- バックオフィス(法務 / 経理)
- システム部
- 社外顧客
```

スイムレーンの色配置は `prompts/base-prompt.md` の §② を参考に。

## プロセス番号・課題番号のプレフィックスを変える

業界によっては `P-XXX` ではなく `OP-XXX`(Operation)、`C-XXX` ではなく `ISS-XXX`(Issue)などにしたい場合があります。
`SKILL.md` の「採番ルール」セクションを直接書き換えてください。

```markdown
採番ルール (カスタム):
- OP-XXX: 時系列順 (Operation の頭文字、デフォルトの P-XXX 相当)
- DOC-XXX: 使用順序 (Document、デフォルトの F-XXX 相当)
- SYS-XXX: 実行順序 (System、デフォルトの S-XXX 相当)
- ISS-XXX: 課題が見えた順序 (Issue、デフォルトの C-XXX 相当)
```

drawio 側のラベルもこれに合わせて変更されます。

## スイムレーンの色を変える

`templates/業務フロー図テンプレート.drawio` を直接編集 → `fillColor=#XXXXXX` を変える、で OK。
変更後の色は `prompts/base-prompt.md` の §② にも反映しておくと、生成時の整合性が保たれます。

## 起動時の挙動を変える

たとえば、「ヒアリングだけは課題発生ポイント分析を強化する」「経営会議は意思決定の系譜を主軸にする」など、用途別に分岐させたい場合は `SKILL.md` の「## このスキルの使い方」セクションに条件分岐を書き加えます。

```markdown
## このスキルの使い方

ユーザーの起動キーワードに応じて、出力の重み付けを変える:

- 「ヒアリング」系キーワード → §5 課題発生ポイント分析 を **倍の粒度** で書く
- 「経営会議」系キーワード → 意思決定の系譜セクションを追加
- 「業務改善」系キーワード → ボトルネック分析を強化
```

Claude Code が SKILL.md を再読すれば、すぐ反映されます。

## カスタマイズ後の保存

GitHub からクローンしたままだと、`git pull` で自分の変更が上書きされる可能性があります。
ローカルブランチで管理するのがおすすめです。

```bash
cd .claude/skills/business-flow-architect
git checkout -b my-customizations
# 編集
git commit -am "trigger keywords をうちの語彙に変更"
```

新版が来たら:

```bash
git checkout main
git pull
git checkout my-customizations
git rebase main   # or merge
```
