# OLAMIP 快速入门

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

这份快速入门将展示如何以最简单的方式在网站上实现 OLAMIP。

## 1. 创建 `olamip.json`

在你的网站根目录放置一个有效的 `olamip.json` 文件：

`https://yourdomain.com/olamip.json`

先从最重要页面的一个小而干净的快照开始。

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {
    "name": "Example Site",
    "type": "blog",
    "canonical_description": "A website about technology and product updates.",
    "tags": ["technology", "blog"]
  },
  "content": {
    "overview": {
      "summary": "A technology blog covering product news, tutorials, and opinion pieces."
    },
    "sections": [
      {
        "title": "Blog",
        "summary": "Articles and guides.",
        "url": "https://yourdomain.com/blog/",
        "section_type": "blog_category",
        "entries": [
          {
            "title": "Welcome to the Blog",
            "summary": "An introduction to the site’s content and purpose.",
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

## 2. 在 HTML 中添加发现标签

将以下内容添加到首页和关键页面的 `<head>` 部分：

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## 3. 保持摘要简洁

撰写简短、事实性的摘要，说明页面内容以及其重要性。避免营销语言、重复文本和模糊描述。

好的示例：

- “长曝光摄影入门指南。”
- “企业计划的产品详情和定价。”

避免：

- “互联网上最好的页面。”
- “你一定会喜欢的精彩内容。”

## 4. 使用正确的内容类型

尽量选择最具体的 `content_type`：

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

## 5. 使用 section 组织内容

将 `sections` 用于内容分组，将 `entries` 用于单独页面。如有需要，可添加 `subsections` 以构建更深层的结构。

示例：

- Section: Blog
- Subsection: Tutorials
- Entry: How to Use OLAMIP

## 6. 有意地设置 priority

使用：

- `high`：最重要的页面。
- `medium`：普通页面。
- `low`：小众或旧页面。

不要把所有内容都标为 `high`。

## 7. 使用 policy 控制 AI 摄取

`policy` 字段会告诉 AI 系统是否允许摄取某个 section、subsection 或 entry。

- 使用 `"allow"` 表示明确允许摄取。
- 使用 `"forbid"` 表示告诉 AI 系统避开某个特定 section、subsection 或 entry。

如果省略 `policy` 字段，OLAMIP 会继承最近祖先的 policy。如果没有任何祖先定义 policy，则默认生效值为 `"allow"`。对于大多数网站，你可以不写 `policy`，只在想让 AI 系统跳过内容时使用 `"forbid"`。

示例：

- 跳过整个 section：

  ```json
  {
    "title": "Deprecated Guides",
    "summary": "Outdated content, not for AI.",
    "url": "https://yourdomain.com/old-guides/",
    "section_type": "doc_category",
    "policy": "forbid"
  }
  ```

- 跳过单个页面：

  ```json
  {
    "title": "Internal Test Page",
    "summary": "Page used for staging only.",
    "url": "https://yourdomain.com/test/",
    "content_type": "page",
    "policy": "forbid"
  }
  ```

## 8. 添加语言元数据

使用 BCP-47 语言代码，例如：

- `en`
- `es`
- `fr`
- `de`
- `pt-BR`
- `zh-CN`

可根据需要在文件级、section 级或 entry 级设置语言。

## 9. 定期更新文件

每当你添加、修改或删除重要页面时，都应更新 `olamip.json`。

如果你的网站变化频繁，也可以在同一目录中维护 `olamip-delta.json`，这样 AI 系统就能在完整刷新之间保持最新。

## 10. 发布前验证

在正式上线前，请确保：

- JSON 有效。
- URL 是绝对地址。
- 必需字段完整。
- 摘要清晰且长度在推荐范围内。
- 标签已标准化且一致。
- 文件从根路径提供。

## 最小实现清单

- [ ] 创建 `olamip.json`。
- [ ] 将其托管在网站根目录。
- [ ] 在 HTML `<head>` 中添加发现标签。
- [ ] 为关键内容添加 sections 和 entries。
- [ ] 添加 metadata 和 language 字段。
- [ ] 保持摘要简短且准确。
- [ ] 每当网站变化时更新它。
- [ ] 如果需要增量更新，添加 `olamip-delta.json`。

## 建议的起点

如果你不确定从哪里开始，可以先只为最重要的页面实现 OLAMIP：

- 首页。
- 博客索引页。
- 最佳文章。
- 关键产品或服务页面。
- 重要文档页面。

然后再逐步扩展。
