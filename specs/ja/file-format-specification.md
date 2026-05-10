# OLAMIP ファイル形式仕様

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## 概要

OLAMIP ファイル（`/olamip.json`）は、ウェブサイト内の最も重要なページの要約を提供する構造化 JSON ドキュメントです。大規模言語モデル（LLM）がコンテンツを明確かつ正確に理解し、優先順位付けし、活用できるように設計されています。

## ファイルの場所

OLAMIP ファイルはドメインのルートに配置する必要があります：

`https://yourdomain.com/olamip.json`

## OLAMIP ファイルの場所の宣言

システムが確実に OLAMIP ファイルを検出できるように、サイトの `<head>` セクションに `<link>` タグと `<meta>` タグの両方を追加してください。

### 主な検出方法：`<link rel="olamip">`

- 標準的な方法：クローラーはすでに `<link>` タグをスキャンしています。
- 機械可読性：ページと OLAMIP ファイルの正式な関係を定義します。
- 相互運用性：既存の Web 標準と互換性があります。

### 代替検出方法：`<meta name="olamip-location">`

- 人間にとって分かりやすい。
- 一部のツールとの互換性。
- バックアップとして機能。

### 推奨実装

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## ファイル構造

UTF‑8 の有効な JSON であり、以下を含む必要があります：

- `protocol`
- `version`
- `identity`
- `content`
- `metadata`

### 構造例

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {},
  "content": {},
  "metadata": {}
}
```

## Identity オブジェクト

| フィールド | 型 | 必須 | 説明 |
|---|---|---:|---|
| `name` | `string` | はい | サイトまたは組織名 |
| `type` | `string` | はい | 種類（例：company、blog） |
| `canonical_description` | `string` | はい | サイトの説明 |
| `tags` | `array<string>` | いいえ | キーワード |

## Content オブジェクト

以下を含みます：

- `overview`
- `sections`

### Overview

| フィールド | 型 | 必須 | 説明 |
|---|---|---:|---|
| `summary` | `string` | はい | サイトの概要 |

## Section 仕様

コンテンツのカテゴリを表します。

### フィールド

| フィールド | 型 | 必須 | 説明 |
|---|---|---:|---|
| `title` | `string` | はい | タイトル |
| `summary` | `string` | はい | 説明 |
| `url` | `string` | はい | URL |
| `section_type` | `string` | はい | 種類 |
| `policy` | `string` | いいえ | `"allow"` または `"forbid"` |
| `entries` | `array<Entry>` | はい | エントリ |
| `subsections` | `array<Section>` | いいえ | サブセクション |

## ポリシー

- `"allow"`：許可
- `"forbid"`：禁止

デフォルト：`"allow"`

### 継承ルール

1. Entry
2. Subsection
3. Section
4. デフォルト

## Entry オブジェクト

最小単位のコンテンツ。

### フィールド

| フィールド | 型 | 必須 | 説明 |
|---|---|---:|---|
| `title` | `string` | はい | タイトル |
| `summary` | `string` | はい | 概要 |
| `url` | `string` | はい | 絶対 URL |
| `content_type` | `string` | はい | コンテンツ種別 |

## コンテンツタイプ

- `page`
- `blog_article`
- `news_article`
- `product`
- `doc_page`
- `research_paper`

## 配列

例：

```json
"tags": ["ai", "search", "llm"]
```

## Metadata

```json
{
  "last_updated": "2026-01-21",
  "language": "ja"
}
```

## 優先度

- `"high"`
- `"medium"`
- `"low"`

## 多言語対応

BCP‑47 を使用：

- `ja`
- `en`
- `zh-CN`

## 検証ルール

- 有効な JSON
- 絶対 URL を使用
- 末尾カンマ禁止

## タグ

ルール：

- 小文字
- 単語形式
- ハイフン使用

例：

- `machine-learning`
- `data-science`

## バージョニング

- 未知のフィールドは無視
- 最新仕様に準拠

## セマンティック整合性

OLAMIP は schema.org、サイトマップ、クローラーを補完し、AI に最適化された構造化レイヤーを提供します。
