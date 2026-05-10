# OLAMIP-DELTA ファイル形式仕様書

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

OLAMIP-DELTA プロトコルは、`olamip.json` に対する増分更新のための、JSONベースのデルタ（差分）ファイル形式を定義します。この仕様は、構造化データ、HTML、およびサイトマップ形式の公開に精通しているウェブマスターおよび実装者を対象としています。

## 1. 概要

`olamip.json` は、サイトの構造とコンテンツの正式かつ完全なスナップショット表現です。一方、`olamip-delta.json` はオプションのコンパニオンファイルであり、前回の更新以降のスナップショットへの変更内容を記述します。

これらを組み合わせることで、マシンが解釈可能な変更ストリームを形成します。

- `olamip.json`: 完全な構造化スナップショット。
- `olamip-delta.json`: 増分変更ログ。

---

## 2. コア・コンセプト

- **Entry（エントリー）** — 末端レベルのコンテンツ項目（例：ブログ記事、製品ページ、ドキュメントページ）。
- **Section / Subsection（セクション / サブセクション）** — 階層的なグループ化（例：ブログのカテゴリ、製品コレクション、プロジェクトグループ）。
- **Operation（オペレーション）** — `added`（追加）、`updated`（更新）、`removed`（削除）のいずれか。
- **Delta（デルタ）** — `added`、`updated`、`removed` を含む、日付単位の変更セット。

---

## 3. ファイルの場所と検出

### 3.1 ファイルの場所

olamip.json は、ドメインのルートに配置する必要があります。
https://yourdomain.com/olamip.json

olamip-delta.json は、同じディレクトリに配置する必要があります。
https://yourdomain.com/olamip-delta.json

バージョン管理されたデルタファイル（オプション）も同じディレクトリに配置します。
/olamip.json
/olamip-delta-YYYY-MM-DD.json

### 3.2 検出メカニズム

ウェブマスターは、サイトの <head> 内で以下の両方のタグを使用して olamip.json の場所を宣言する必要があります。

<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">

---

## 4. トップレベル構造

トップレベルでは、olamip-delta.json は以下のフィールドを持つ JSON オブジェクトです。

{
  "protocol": "OLAMIP-DELTA",
  "version": "1.0.0",
  "window_days": 7,
  "last_updated": "2026-04-28",
  "deltas": [
    {
      "date": "2026-04-27",
      "added": [
        {
          "title": "Example entry",
          "url": "https://example.com/page/",
          "summary": "...",
          "content_type": "blog_article"
        }
      ],
      "updated": [
        {
          "url": "https://example.com/page/",
          "summary": "更新された概要"
        }
      ],
      "removed": [
        {
          "url": "https://example.com/removed-page/"
        }
      ]
    }
  ]
}

### 4.1 必須のトップレベルフィールド

- protocol (string, 必須): "OLAMIP-DELTA" である必要があります。
- version (string, 必須): 文書化されたバージョン体系（例: "1.0.0"）に一致する必要があります。
- window_days (integer, オプション): ローリングウィンドウの日数。
- last_updated (string, 必須): このファイルが最後に変更された日付 (ISO-8601形式)。
- deltas (array, 必須): date、added、updated、removed を持つデルタオブジェクトの配列。

---

## 5. デルタオブジェクトの構造

解釈の明確さと一貫性を保つため、ウェブマスターは、各デルタオブジェクトに 3 つの操作フィールド（added、updated、removed）すべてを含める必要があります（空の場合でも必須）。

---

## 6. オペレーション

### 6.1 added
added 内の項目は完全なオブジェクトである必要があります。フィールドは OLAMIP メイン仕様に準拠する必要があります。

### 6.2 updated
項目には url を含める必要があり、部分的なオブジェクト（変更されたフィールドのみ）または完全なオブジェクトのいずれかになります。

### 6.3 removed
削除される項目は最小限で、アイデンティティのみを含みます。
{ "url": "https://example.com/page/to-remove/" }

---

## 7. URL とオブジェクトのアイデンティティ
OLAMIP-DELTA は、正規 URL をプライマリ・アイデンティティとして使用します。URL を変更する場合は、インプレース更新ではなく、削除 (remove) の後に追加 (add) を行う必要があります。

---

## 8. ローリングデルタウィンドウ（推奨）
このモデルでは、ファイルに過去数日間（通常 7〜30 日間）の変更が含まれます。これにより、クローラが遅延した場合でも AI システムが更新を見逃すのを防ぎます。

---

## 9. バージョン管理されたデルタ（代替案）
日付ごとに個別のファイル（olamip-delta-YYYY-MM-DD.json）を使用することもできます。AI システムはディレクトリ内のファイルを検出し、時系列順に適用する必要があります。

---

## 10. 構造の変更：セクションとサブセクション
エントリーと同様のデルタルールがセクションとサブセクションにも適用されます。セクションが削除されると、その配下にあるすべての要素（サブセクションとエントリー）も論理的に削除されたものとみなされます。

---

## 11. 適合性

### 11.1 MUST（必須）ルール
- protocol は "OLAMIP-DELTA" であること。
- added、updated、removed フィールドをすべてのデルタオブジェクトに含めること。
- URL は絶対パスかつ正規 URL であること。
- deltas 配列は日付の昇順でソートされていること。

---

## 12. ベストプラクティス
1. CMS またはデプロイパイプラインを通じてデルタ生成を自動化する。
2. olamip.json を常に最新かつ正式なものとして維持する。
3. デルタ内での不必要な混乱を避けるため、安定した正規 URL を使用する。

---

## 13. 関連リソース
- OLAMIP ファイル形式仕様書: https://olamip.org/file-format-specification/
- よくある質問 (FAQ): https://olamip.org/frequently-asked-questions/
- なぜ OLAMIP が LLMs.txt より優れた標準なのか: https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/

---
*この文書は、本番環境での olamip-delta.json ファイルの生成、検証、および利用のリファレンスとして機能します。*
