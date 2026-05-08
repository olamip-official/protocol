text
# OLAMIP の概要

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP は、Web サイトが AI システムと明確かつ意図的にコミュニケーションするためのオープンスタンダードです。ノイズの多い HTML、CSS、JavaScript から大規模言語モデル（LLM）に意味を推測させるのではなく、OLAMIP はサイトの最も重要なコンテンツを、クリーンで構造化された、機械可読な形で表現します。

この文書では、OLAMIP の目的、設計思想、ファイル形式、更新モデルについて、高レベルで紹介します。

---

## 1. OLAMIP が作られた理由

現代の Web サイトは、人間のブラウザ向けに作られており、AI 向けではありません。そのため、LLM は次のような要素の扱いに苦労します。

- レイアウトの重い HTML  
- JavaScript でレンダリングされるコンテンツ  
- ナビゲーションメニュー、広告、定型文  
- 重複ページや関連性の低いページ  

LLM は *すでに* あなたのサイトをクロールしていますが、しばしばそれを誤って解釈します。OLAMIP は、AI システムに必要なものを正確に提供することで、この問題を解決します。

- 軽量な JSON ファイル（`/olamip.json`）  
- 人間が整理したクリーンな要約  
- 構造化されたメタデータ  
- 正規 URL  
- section、subsection、entry の明確な階層  

OLAMIP を採用することで、AI があなたのコンテンツをどのように理解し、表現するかを制御できます。

---

## 2. 中核となる思想: 人間にやさしく、機械にやさしい

OLAMIP は、人間と AI の間のコミュニケーションの橋として設計されています。プロトコルのすべての要素は、次の両方を満たす必要があります。

| Principle | Human‑Friendly | Machine‑Friendly |
|-----------|----------------|------------------|
| File Format | 書きやすく、検証しやすい | 厳密で予測可能な JSON |
| Field Names | 直感的で説明的 | 小文字で、schema に準拠 |
| Summaries | 明確でブランドに沿った言語 | 簡潔で、事実に基づき、意味情報が豊富 |
| Priority System | シンプルなラベル（`"high"`、`"medium"`、`"low"`） | ランキング重み付けに対応可能 |
| Documentation | 平易な言葉のガイド | JSON schema、検証ルール |
| Tooling | ジェネレーター、CMS プラグイン | CLI ツール、パーサー、テストスイート |

- **人間にとって:** OLAMIP は、深い技術知識がなくても簡単に導入できる必要があります。  
- **機械にとって:** OLAMIP は、構造化され、予測可能で、曖昧さがない必要があります。

---

## 3. OLAMIP ファイル（`/olamip.json`）

OLAMIP ファイルは、次の場所でホストされる構造化 JSON ドキュメントです。

`https://yourdomain.com/olamip.json`

含まれる内容は次のとおりです。

- `protocol` — `"OLAMIP"` でなければなりません  
- `version` — プロトコルのバージョン  
- `identity` — あなたが誰か  
- `content` — サイトの構造化された階層  
- `metadata` — 言語、最終更新日、その他の全体メタデータ  

### 3.1 Discovery タグ

AI システムが OLAMIP ファイルを確実に見つけられるように、次の両方を含めてください。

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

両方を使うことで、冗長性、互換性、将来への備えが得られます。

---

## 4. ファイル構造

### 4.1 Identity オブジェクト

Web サイトまたは組織を表します。

- `name` — 必須  
- `type` — 必須  
- `canonical_description` — 必須  
- `tags` — 任意  

### 4.2 Content オブジェクト

次を含みます。

- `overview`  
- `sections`  
- 任意の `subsections`  
- `entries`（最も細かいコンテンツ単位）  

これにより、無制限のネスト深度をサポートします。

### 4.3 Sections

Section は関連するコンテンツをまとめます。必須フィールドは次のとおりです。

- `title`  
- `summary`  
- `url`  
- `section_type`  

任意フィールドは次のとおりです。

- `policy`（`"allow"` または `"forbid"`）  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**policy の継承:**  
省略された場合、policy は親から継承されます。デフォルトは `"allow"` です。

### 4.4 Entries

Entry は次のような個別コンテンツ項目を表します。

- ブログ記事  
- ニュース記事  
- 製品  
- ドキュメントページ  
- 研究論文  
- メディア項目  

必須フィールド:

- `title`  
- `summary`  
- `url`  
- `content_type`  

任意フィールド:

- `policy`, `tags`, `priority`, `published`, `language`, `metadata`  

URL は、コンテンツを一意に識別し、AI システムがページを検証、重複排除、相互参照できるようにするために必要です。

---

## 5. メタデータと言語サポート

metadata オブジェクトには次のような値を含めることができます。

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### 多言語サポート

言語は次のレベルで定義できます。

- ファイルレベル  
- section レベル  
- entry レベル  

BCP‑47 言語コード（例: `en`、`es`、`pt-BR`、`zh-CN`）を使用してください。

これは AI システムが次のことを行うのに役立ちます。

- 正しいトークナイザーを選ぶ  
- 言語の混在を避ける  
- 検索精度を向上させる  
- ハルシネーションを減らす  

---

## 6. タグと優先度

### 6.1 Tags

タグは軽量な意味の手がかりを提供します。次の条件を満たす必要があります。

- 小文字  
- 1 語  
- ASCII  
- 複数語の概念にはハイフンを使用  
- すべてのエントリで一貫していること  

例:

| Concept | Valid Tag |
|---------|-----------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

タグは AI システムがコンテンツをより正確にクラスタリングし、曖昧性を解消し、検索するのに役立ちます。

### 6.2 Priority

値:

- `high` — 旗艦コンテンツ  
- `medium` — デフォルト  
- `low` — ニッチまたは古いコンテンツ  

`high` は控えめに使用してください（全エントリの 5〜10% 程度）。

---

## 7. 差分更新（`olamip-delta.json`）

Web サイトは常に変化します。OLAMIP は、任意の補助ファイルを通じて増分更新をサポートします。

`/olamip-delta.json`

このファイルには次のものだけが含まれます。

- 追加されたエントリ  
- 更新されたエントリ  
- 削除された URL  

### 7.1 差分更新が重要な理由

- AI システムは全ファイルを再処理せずに最新状態を維持できます。  
- 大規模なカタログでも管理しやすいままです。  
- 更新がすばやく反映されます。  
- 変更の取りこぼしを防げます。  

### 7.2 ローリング差分ウィンドウ（推奨）

過去 7〜30 日分の差分を 1 つのファイルにまとめてください。  
AI システムはそれらを時系列で適用し、同期を保ちます。

### 7.3 バージョン付き差分ファイル（代替）

別々のファイルを公開します。

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...  

ニュース、e コマース、ドキュメントなど、高頻度更新サイトに適しています。

### 7.4 Discovery

タグ `<link>` と `<meta>` が必要なのはメインの OLAMIP ファイルだけです。  
AI システムは同じディレクトリ内の差分ファイルを自動的に探します。

---

## 8. OLAMIP と従来のサイトマップ

- XML Sitemap: 「ここに私のページがあります。」  
- OLAMIP: 「ここに私のサイトの意味があります。」  

サイトマップは URL を列挙します。  
OLAMIP は次を提供します。

- 要約  
- コンテンツタイプ  
- section 階層  
- タグ  
- メタデータ  
- 優先度  
- 取り込みポリシー  

schema.org と組み合わせることで、OLAMIP は AI システムにあなたのサイトの完全で人間が整理した理解を提供します。

---

## 9. よくある質問（要約）

### OLAMIP はプライベート情報を公開しますか？

いいえ。公開されているコンテンツのみを要約します。

### 競合他社は OLAMIP ファイルを使えますか？

公開ページを使えるのと同じ程度以上には使えません。

### OLAMIP ファイルはどのくらい大きくできますか？

通常は 1〜10 MB です。  
3,000 件以上でも正常で安全です。

### どのくらいの頻度で更新すべきですか？

コンテンツが変わるたびに更新してください。AI 向けサイトマップとして扱ってください。

### 差分を使う場合、メインファイルも更新する必要がありますか？

はい。`olamip.json` は常に権威あるスナップショットです。

### LLM は本当にこれを読みますか？

はい。それがこのプロトコルの目的です。  
次のようにテストできます。

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. まとめ

OLAMIP は、あなたの Web サイトを機械可読な知識マップへと変換します。

- `olamip.json` — 完全で構造化されたスナップショット  
- `olamip-delta.json` — 増分変更ログ  
- Sections、subsections、entries — 明確な階層  
- 要約、タグ、優先度 — 人間が整理した意味  
- Discovery タグ — 信頼できる取り込み  
- 多言語サポート — グローバル対応  

OLAMIP を採用することで、AI システムがあなたの意図どおりにコンテンツを理解し、正確に、一貫して、そして大規模に扱えるようになります。
