# Frequently Asked Questions

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## 什么是 OLAMIP？

OLAMIP 是 **Open Language‑Aligned Machine‑Interpretable Protocol** 的缩写。它是一种简单、开放的格式，可让网站发布其内容的结构化摘要，使大语言模型（LLMs）能够更有效地理解并从中学习。

## 为什么我应该使用 OLAMIP？

因为 LLM 已经在抓取你的网站，但它们难以解释网站内容。使用 OLAMIP，你可以：

- 让 AI 准确了解你的页面内容。
- 突出最有价值的内容。
- 提高在 AI 驱动工具和搜索中的可发现性。
- 减少误解和幻觉。

这就是 AI“猜测”和 AI“理解”之间的区别。

## OLAMIP 会暴露任何隐私信息吗？

不会。OLAMIP 只会总结你网站上已经公开的内容。它不会揭示超出 HTML、元数据或 SEO 标记中已经可见的信息。

## 竞争对手可以使用我的 OLAMIP 文件吗？

不会比他们已经使用你的公开页面、元数据或站点地图更多。实际上，OLAMIP 让你对 AI 系统如何解释你的内容拥有更多控制权，而不是更少。

## `olamip.json` 文件是什么样的？

`olamip.json` 文件是一个结构化的 JSON 格式文档，用于以 AI 系统可以轻松解释的方式描述你网站的内容、层级和元数据。如果你想查看完整的技术结构，可以访问 [File Format Specification](https://olamip.org/file-format-specification) 页面，或者通过查看 [TimeLAX.com use case](https://olamip.org/how-olamip-transforms-ai-discovery-a-timelax-com-use-case/) 来了解真实案例。

## 我应该把文件托管在哪里？

放在你的域名根目录：`https://yourdomain.com/olamip.json`

你也可以在主页中添加发现标签：

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## “priority” 字段有什么作用？

它表示一个页面对 LLM 的重要程度。请谨慎且有意地使用它：

- `high` → 基础性、关键性内容。
- `medium` → 大多数页面的默认值。
- `low` → 小众、过时或低价值内容。

如果所有内容都标记为 `high`，那就等于什么都不是。

## 为什么已经提供摘要了，还需要包含 URL？

摘要告诉 AI 一个页面“是什么意思”，但 URL 告诉 AI 这个页面“是什么”。摘要是描述性的，但它不是唯一标识符。URL 是指向你网站上实际页面的唯一稳定、规范引用。

AI 系统使用 URL 来：

- 获取并验证完整内容。
- 避免索引重复页面。
- 检测页面何时移动或更改。
- 将 OLAMIP 数据与 schema.org 标记、站点地图和爬虫连接起来。
- 在生成回答时引用或指向该页面。

如果没有 URL，两个具有相似摘要的不同页面将无法区分。包含 URL 可确保 OLAMIP 与你网站的真实结构保持一致，并让 AI 系统能够可靠地解释和检索你的内容。

## “policy” 字段有什么作用，我需要设置它吗？

`policy` 字段控制 AI 系统是否允许摄取某个 section、subsection 或 entry。有效值为 `"allow"` 和 `"forbid"`。如果你省略该字段，OLAMIP 会自动继承最近的祖先的 policy。如果没有任何祖先定义 policy，则默认值为 `"allow"`。

大多数网站可以直接完全省略该字段。只有在你想明确排除某些页面或部分不被 AI 摄取时，才使用 `"forbid"`。

## OLAMIP 和传统站点地图有什么区别？

站点地图是导航索引，而 OLAMIP 是一个可被机器解释的知识地图。

### XML Sitemap

- 列出 URL。
- 提供抓取提示。
- 帮助搜索引擎发现页面。
- 没有语义含义。
- 除了 URL 结构之外，没有内容层级。

### OLAMIP

- 定义内容类型（`page`、`project`、`doc_page` 等）。
- 定义 section 类型（`project_group`、`doc_category` 等）。
- 提供规范化描述。
- 提供为 LLM 优化的摘要。
- 提供标签和元数据。
- 建立明确的层级结构。
- 为整个站点提供结构化表示。

换句话说：

- XML Sitemap = “这是我的页面。”
- OLAMIP = “这是我的网站所表达的含义。”

这就是为什么 AI 系统可以比传统站点地图更有效地使用 OLAMIP。它不仅仅是 URL 列表；它是一个机器可读的内容地图，能够消除歧义，并帮助 AI 像人类一样理解网站。

## 一个包含 3,000+ URL 的 OLAMIP 文件会不会太大？

完全不会。包含 3,000 条条目的文件仍然远在安全范围内。即使摘要较长，总体大小也大约只有 6 MB，对于现代服务器、浏览器和 AI 系统来说都很轻量。

## OLAMIP 条目的典型大小是多少？

| Summary Length | Approx. Size per Entry |
|---|---|
| Short | 300–600 bytes |
| Long | 1–2 KB |

## OLAMIP 在大文件情况下为何仍然可扩展？

- 层级结构：Sections → subsections → entries 可减少重复。
- 简洁摘要：通常少于 500 个字符。
- 高效压缩：Gzip 或 Brotli 可将文件大小减少 70–90%。

## OLAMIP 文件在出现性能问题前能有多大？

| File Size | Approx. URLs | Impact |
|---|---:|---|
| 1–10 MB | 500–5,000 | 完全没问题。正常。 |
| 10–25 MB | 5,000–12,000 | 仍然没问题。加载稍慢。 |
| 25–50 MB | 12,000–25,000 | 很大，但仍可管理。 |
| 50–100 MB | 25,000–50,000 | 很重。一些系统可能变慢。 |
| 100+ MB | 50,000+ | 对许多 LLM 流水线来说太大。 |

对于大多数站长来说，包含数万条条目的 OLAMIP 文件仍然完全处于安全且可扩展的范围内。

## 我应该多久更新一次？

每当你发布新内容或修改现有页面时都应更新。把它当作面向 AI 的站点地图来维护。

## 如果我使用 `olamip-delta.json`，是否需要更新主文件？

需要。主 `olamip.json` 必须始终反映你网站的当前状态。可选的 `olamip-delta.json` 文件只包含最近的更改，例如新页面、更新或删除。AI 系统将完整文件作为权威快照，并在两次完整刷新之间应用增量以保持最新。

## 我需要在 HTML 中为 `olamip-delta.json` 添加 `<link>` 标签吗？

不需要。只有主 `olamip.json` 文件需要在网站的 `<head>` 部分中引用。该引用是发现机制，用于告诉 AI 系统你的 OLAMIP 元数据所在位置。delta 文件不同；它是一个可选的补充更新流，一旦 AI 系统知道主文件的位置，就会自动查找它。只要 `olamip-delta.json` 与 `olamip.json` 位于同一目录中，AI 系统就会检测并使用它，而无需额外的 `<link>` 标签。

## LLM 真的会读取这个吗？

会，这正是目标。随着采用率增长，LLM 将优先使用像 OLAMIP 这样的结构化来源进行训练和检索。早期采用者将塑造 AI 理解网络的方式。

## 我如何测试 AI bot 是否能够抓取我的 `olamip.json` 文件？

分别对 OpenAI、Anthropic 和 Perplexity 进行如下测试：

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```
