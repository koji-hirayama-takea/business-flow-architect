# 03_ drawio MCP フォーマット指定ルール

drawio MCP を経由して `.drawio` ファイルを生成 → ブラウザでプレビューするための XML 規約。Claude Code が ② 業務フロー図を生成する際の出力フォーマット。

---

## drawio MCP の概要

| ツール | 用途 |
|---|---|
| `mcp__drawio__open_drawio_xml` | drawio XML 文字列を渡すと drawio.com でブラウザプレビュー |
| `mcp__drawio__open_drawio_mermaid` | Mermaid 構文を渡すと drawio.com でプレビュー (簡易図向き) |
| `mcp__drawio__open_drawio_csv` | CSV 形式 (フロー定義) を渡すとプレビュー |

**業務フロー図には `open_drawio_xml` を使う** (スイムレーン + 多種図形が必要なため)。

---

## XML 出力先

```bash
./業務フロー/[プロセス名]_業務フロー図_v[版数]_[YYYYMMDD].drawio
```

例: `./業務フロー/メンバー状況把握_業務フロー図_v1.0_20260527.drawio`

---

## XML 骨格(コピー用)

```xml
<mxfile host="65bd71144e">
  <diagram name="[プロセス名]_業務フロー図" id="bp001">
    <mxGraphModel dx="1280" dy="1200" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="1654" pageHeight="2339" math="0" shadow="0">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>

        <!-- タイトル -->
        <mxCell id="title" value="[プロセス名] 業務フロー図 v1.0" style="text;fontSize=18;fontStyle=1;align=center" vertex="1" parent="1">
          <mxGeometry x="40" y="20" width="1500" height="40" as="geometry"/>
        </mxCell>

        <!-- ⚠️ 重要: mxCell は入れ子にしない。全て <root> 直下のフラット兄弟、親子関係は parent="..." 属性で表現する -->

        <!-- スイムレーンコンテナ (9 レーン) -->
        <mxCell id="swim-container" value="" style="swimlane;startSize=0;horizontal=0;swimlaneFillColor=#ffffff" vertex="1" parent="1">
          <mxGeometry x="40" y="80" width="1530" height="900" as="geometry"/>
        </mxCell>

        <!-- L1: 社外 (parent=swim-container) -->
        <mxCell id="lane-soto" value="社外 (取引先/顧客)" style="swimlane;fillColor=#F5F5F5;startSize=30" vertex="1" parent="swim-container">
          <mxGeometry x="0" y="0" width="170" height="900" as="geometry"/>
        </mxCell>

        <!-- L2: フロント担当者 -->
        <mxCell id="lane-front-tan" value="フロント担当者" style="swimlane;fillColor=#E6F3FF;startSize=30" vertex="1" parent="swim-container">
          <mxGeometry x="170" y="0" width="170" height="900" as="geometry"/>
        </mxCell>

        <!-- L3-L9: フロント承認者 / ミドル担当者 / ミドル承認者 / バック担当者 / バック承認者 / 社内システム / 社外サービス を同様にフラット配置 (parent="swim-container") -->
        <!-- 省略 — templates/業務フロー図テンプレート.drawio を参照 -->

        <!-- 開始ノード (parent="1") -->
        <mxCell id="start" value="開始" style="ellipse;fillColor=#D5E8D4;strokeColor=#82B366" vertex="1" parent="1">
          <mxGeometry x="100" y="120" width="100" height="40" as="geometry"/>
        </mxCell>

        <!-- プロセス P-001 (whiteSpace=wrap;html=1 を必ず入れる、改行 &#xa; を正しく表示するため) -->
        <mxCell id="p001" value="P-001&#xa;[プロセス名]" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#DAEEFF;strokeColor=#6C8EBF" vertex="1" parent="1">
          <mxGeometry x="270" y="180" width="120" height="60" as="geometry"/>
        </mxCell>

        <!-- ファイル F-001 (P-001 の右側に付箋) -->
        <mxCell id="f001" value="F-001&#xa;[ファイル名]" style="shape=note;whiteSpace=wrap;html=1;fillColor=#FFF2CC;strokeColor=#D6B656" vertex="1" parent="1">
          <mxGeometry x="400" y="180" width="100" height="50" as="geometry"/>
        </mxCell>

        <!-- 課題発生ポイント C-001 (P-001 の左下に吹き出し) -->
        <mxCell id="c001" value="⚠️ C-001&#xa;[課題要約]" style="shape=callout;whiteSpace=wrap;html=1;fillColor=#FFE6E6;strokeColor=#D32F2F;fontStyle=1" vertex="1" parent="1">
          <mxGeometry x="120" y="260" width="140" height="50" as="geometry"/>
        </mxCell>

        <!-- 承認判断 P-002 (ひし形) -->
        <mxCell id="p002" value="P-002&#xa;[判断名]" style="rhombus;whiteSpace=wrap;html=1;fillColor=#FFE6E6;strokeColor=#B85450" vertex="1" parent="1">
          <mxGeometry x="430" y="290" width="120" height="80" as="geometry"/>
        </mxCell>

        <!-- システム処理 S-001 (四角) -->
        <mxCell id="s001" value="S-001&#xa;[システム処理]" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#E8D8FF;strokeColor=#9673A6" vertex="1" parent="1">
          <mxGeometry x="1220" y="290" width="120" height="60" as="geometry"/>
        </mxCell>

        <!-- 終了ノード -->
        <mxCell id="end" value="終了" style="ellipse;fillColor=#F8CECC;strokeColor=#B85450" vertex="1" parent="1">
          <mxGeometry x="100" y="800" width="100" height="40" as="geometry"/>
        </mxCell>

        <!-- フロー線: 通常 (実線) -->
        <mxCell id="flow1" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1" edge="1" parent="1" source="start" target="p001">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>
        <mxCell id="flow2" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1" edge="1" parent="1" source="p001" target="p002">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- フロー線: システム連携 (破線) -->
        <mxCell id="flow-sys" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;dashed=1" edge="1" parent="1" source="p002" target="s001">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- 凡例 -->
        <mxCell id="legend" value="【凡例】&#xa;⚪ 開始/終了 (楕円)&#xa;▢ 業務プロセス (角丸四角)&#xa;◇ 承認・判断 (ひし形)&#xa;▭ システム処理 (四角)&#xa;📝 ファイル・データ (付箋)&#xa;⚠️ 課題発生ポイント (吹き出し)&#xa;→ 通常フロー (実線)&#xa;⋯→ システム連携 (破線)" style="text;align=left;whiteSpace=wrap;html=1;fillColor=#F5F5F5;strokeColor=#666666" vertex="1" parent="1">
          <mxGeometry x="40" y="1020" width="600" height="160" as="geometry"/>
        </mxCell>

      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## drawio MCP 経由のプレビュー手順

Claude Code 内で:

```
# 1. XML を ./業務フロー/ 配下に書き出す
[Write tool で .drawio ファイルを保存]

# 2. 同じ XML 文字列を drawio MCP に渡す
[mcp__drawio__open_drawio_xml に xml_content を渡す]

# 3. ブラウザが自動で drawio.com を開いて図がプレビューされる
# 4. ブラウザ上で手動微調整 → File > Save で保存
```

---

## 図形要素の使い分け

| 記号 | 用途 | スタイル | 色 |
|---|---|---|---|
| 楕円 | 開始/終了 | `ellipse` | 開始: #D5E8D4 / 終了: #F8CECC |
| 角丸四角 | 業務プロセス | `rounded=1` | #DAEEFF |
| ひし形 | 承認・判断 (分岐) | `rhombus` | #FFE6E6 |
| 四角 | システム処理 | `rounded=0` | #E8D8FF |
| 付箋 | ファイル F-XXX | `shape=note` | #FFF2CC |
| 吹き出し | 課題発生ポイント C-XXX | `shape=callout;strokeColor=#D32F2F` | #FFE6E6 |

---

## フロー線の種類

| 種類 | 用途 | スタイル |
|---|---|---|
| 実線 | 通常のプロセスフロー | `edgeStyle=orthogonalEdgeStyle` |
| 破線 | システム連携 | `edgeStyle=orthogonalEdgeStyle;dashed=1` |

---

## 整合性チェック (Claude Code が生成後に自動実行する項目)

- [ ] 業務記述書 §2 の P-XXX 番号と drawio 内の P-XXX 番号が完全一致
- [ ] 業務記述書 §3 の F-XXX 番号と drawio 内の F-XXX 番号が完全一致
- [ ] 業務記述書 §4 の S-XXX 番号と drawio 内の S-XXX 番号が完全一致
- [ ] **🆕 業務記述書 §5 の C-XXX 番号と drawio 内の課題吹き出し ⚠️ C-XXX が完全一致**
- [ ] 開始ノードが上端、終了ノードが下端に配置されている
- [ ] フロー線がプロセス間を時系列順に繋いでいる
- [ ] スイムレーンは既存 9 種以内 (新規作成していない)

---

## XML 生成時のよくあるエラーと対処

| エラー | 原因 | 対処 |
|---|---|---|
| **`a.push is not a function for mxCell`** | **mxCell を入れ子にしている**(`<mxCell>` の中に他の `<mxCell>` が入っている) | **全 mxCell を `<root>` 直下のフラット兄弟**にして、親子関係は `parent="..."` 属性のみで表現する |
| `drawio MCP が "Invalid XML"` | 改行コードや特殊文字 | `&#xa;` を改行に使う、`<` `>` `&` `"` は実体参照に |
| 改行 (&#xa;) がセルの中で表示されない | style に `whiteSpace=wrap;html=1` が無い | スタイルに `whiteSpace=wrap;html=1` を追加 |
| スイムレーンに要素が乗らない | parent 指定が間違い | レーン内に配置したい場合は `parent="lane-XXX"`、絶対座標で配置したい場合は `parent="1"` |
| 課題吹き出しが見えない | 座標がスイムレーン外 | 該当プロセスの左下 x-50, y+80 を目安に配置 |
| 図がスマホで切れる | pageWidth/pageHeight が小さい | A4 横より大きく (例: 1654x2339) |

### 必読: mxCell 入れ子禁止の徹底ルール

drawio の XML フォーマットでは、`<mxCell>` の中に別の `<mxCell>` を書くと **絶対に開けません**(エラー: `a.push is not a function for mxCell`)。

**NG パターン**:

```xml
<mxCell id="swim-container">
  <mxCell id="lane-soto"/>   <!-- ❌ 入れ子 -->
  <mxCell id="lane-front-tan"/>   <!-- ❌ 入れ子 -->
</mxCell>
```

**OK パターン**(全 mxCell が `<root>` 直下、親子は属性で表現):

```xml
<root>
  <mxCell id="swim-container" parent="1">
    <mxGeometry .../>
  </mxCell>
  <mxCell id="lane-soto" parent="swim-container">
    <mxGeometry .../>
  </mxCell>
  <mxCell id="lane-front-tan" parent="swim-container">
    <mxGeometry .../>
  </mxCell>
</root>
```

drawio MCP / drawio.com のパーサーは、入れ子をエラーにします。AI に XML を生成させるときは、必ずこのルールを守るように指示する。

