# OLAMIP クイックスタート

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

このクイックスタートでは、最も簡単な方法でウェブサイトに OLAMIP を実装する方法を示します。

## 1. `olamip.json` を作成する

サイトのルートに有効な `olamip.json` ファイルを配置します：

`https://yourdomain.com/olamip.json`

最も重要なページの小さくクリーンなスナップショットから始めましょう。

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {
    "name": "Example Site",
    "type": "blog",
    "canonical_description": "技術と製品アップデートに関するウェブサイト。",
    "tags": ["技術", "blog"]
  },
  "content": {
    "overview": {
      "summary": "製品ニュース、チュートリアル、意見記事を扱うテクノロジーブログ。"
    },
    "sections": [
      {
        "title": "ブログ",
        "summary": "記事とガイド。",
        "url": "https://yourdomain.com/blog/",
        "section_type": "blog_category",
        "entries": [
          {
            "title": "ブログへようこそ",
            "summary": "サイトのコンテンツと目的の紹介。",
            "url": "https://yourdomain.com/blog/welcome/",
            "content_type": "blog_article",
            "tags": ["intro", "blog"],
            "priority": "high",
            "language": "en"
          }
        ]
      }
    ]
  },
  "metadata": {
    "last_updated": "2026-04-06",
    "language": "en",
    "source_url": "https://yourdomain.com/",
    "copyright": "© 2026 Example Site"
  }
}
```

## 2. HTML に discovery タグを追加

ホームページと主要ページの `<head>` セクションに以下を追加：

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## 3. 要約を簡潔に保つ

ページの内容と重要性を説明する、短く事実的な要約を書いてください。マーケティング用語、繰り返しテキスト、曖昧な記述は避けてください。

良い例：

- 「長時間露光写真の初心者ガイド。」
- 「エンタープライズプランの製品詳細と価格。」

避ける例：

- 「インターネット最高のページ。」
- 「絶対気に入る素晴らしいコンテンツ。」

## 4. 正しいコンテンツタイプを使用

最も具体的な `content_type` を選択：

- `page`
- `landing_page`
- `legal_page`
- `blog_article`
- `news_article`
- `product`
- `service`
- `doc_page`
- `research_paper`
- `dataset`
- `project`
- `media_item`
- `resource`

## 5. セクションで整理

`sections` をコンテンツグループに、`entries` を個別ページに使用します。必要に応じて、より深い構造を作るために `subsections` を追加。

例：

- Section: ブログ
- Subsection: チュートリアル
- Entry: OLAMIP の使い方

## 6. priority を意図的に設定

使用：

- `high`：最も重要なページ
- `medium`：通常のページ
- `low`：ニッチまたは古いページ

すべてを `high` にしないでください。

## 7. policy で AI 取り込みを制御

`policy` フィールドは、AI システムが section、subsection、entry を取り込めるかどうかを指示します。

- `"allow"` で明示的に取り込みを許可
- `"forbid"` で特定の section、subsection、entry を AI システムにスキップさせる

`policy` フィールドを省略すると、最も近い祖先から policy を継承します。祖先で定義されていない場合は、効果的な policy は `"allow"` になります。ほとんどのサイトでは `policy` を省略し、AI にスキップさせたいコンテンツでのみ `"forbid"` を使用できます。

例：

- セクション全体をスキップ：

  ```json
  {
    "title": "非推奨ガイド",
    "summary": "AI 用ではない古いコンテンツ。",
    "url": "https://yourdomain.com/old-guides/",
    "section_type": "doc_category",
    "policy": "forbid"
  }
  ```

- 単一ページをスキップ：

  ```json
  {
    "title": "内部テストページ",
    "summary": "ステージング専用のページ。",
    "url": "https://yourdomain.com/test/",
    "content_type": "page",
    "policy": "forbid"
  }
  ```

## 8. 言語メタデータを追加

BCP-47 言語コードを使用：

- `en`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`

必要に応じてファイルレベル、セクションレベル、エントリレベルで言語を設定。

## 9. 定期的にファイルを更新

重要なページを追加、変更、削除するたびに `olamip.json` を更新してください。

サイトが頻繁に変更される場合は、同じディレクトリに `olamip-delta.json` も維持し、完全更新の合間に AI システムを最新状態に保ちます。

## 10. 公開前に検証

運用開始前に確認：

- JSON が有効であること
- URL が絶対パスであること
- 必須フィールドが揃っていること
- 要約が明確で推奨長以内であること
- タグが正規化され一貫していること
- ルートパスからファイルが提供されていること

## 最小実装チェックリスト

- [ ] `olamip.json` を作成
- [ ] サイトルートに配置
- [ ] HTML `<head>` に discovery タグを追加
- [ ] 主要コンテンツの sections と entries を含める
- [ ] metadata と language フィールドを追加
- [ ] 要約を短く正確に保つ
- [ ] サイト変更時に更新
- [ ] 増分更新が必要なら `olamip-delta.json` を追加

## 推奨スタート地点

どこから始めるか分からない場合は、まず最重要ページのみで OLAMIP を実装：

- ホームページ
- ブログインデックス
- 最高の記事
- 主要製品・サービスページ
- 重要ドキュメントページ

その後、段階的に拡張してください。
