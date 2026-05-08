# OLAMIP 概述

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## Open Language‑Aligned Machine‑Interpretable Protocol

OLAMIP 是一种开放标准，使网站能够以清晰且有意图的方式与 AI 系统沟通。OLAMIP 不会强迫大型语言模型（LLMs）从杂乱的 HTML、CSS 和 JavaScript 中猜测含义，而是为你网站最重要的内容提供一个干净、结构化、机器可解释的表示。

本文档对 OLAMIP 的目标、理念、文件格式和更新模型提供高层次介绍。

---

## 1. 为什么创建 OLAMIP

现代网站是为浏览器而构建的，而不是为 AI 而构建的。因此，LLMs 往往难以处理以下内容：

- 布局复杂的 HTML  
- 由 JavaScript 渲染的内容  
- 导航菜单、广告和重复性内容  
- 重复或无关页面  

LLMs *已经* 会抓取你的网站，但它们常常会误解网站内容。OLAMIP 通过为 AI 系统提供它们真正需要的内容来解决这个问题：

- 一个轻量级 JSON 文件（`/olamip.json`）  
- 干净、人工整理的摘要  
- 结构化元数据  
- 规范 URL  
- 清晰的章节、子章节和条目层级  

采用 OLAMIP 后，你就能掌控 AI 如何理解和呈现你的内容。

---

## 2. 核心理念：对人友好 + 对机器友好

OLAMIP 被设计为人类与 AI 之间的沟通桥梁。协议的每一部分都必须兼顾以下两点：

| Principle | Human‑Friendly | Machine‑Friendly |
|-----------|----------------|------------------|
| File Format | 易于编写和验证 | 严格、可预测的 JSON |
| Field Names | 直观且不言自明 | 小写、符合 schema 规范 |
| Summaries | 清晰、符合品牌风格的语言 | 简洁、事实性强、语义丰富 |
| Priority System | 简单标签（`"high"`、`"medium"`、`"low"`） | 可映射为排序权重 |
| Documentation | 通俗语言指南 | JSON schemas、验证规则 |
| Tooling | 生成器、CMS 插件 | CLI 工具、解析器、测试套件 |

- **对人类：** OLAMIP 必须易于采用，而不需要深厚的技术背景。  
- **对机器：** OLAMIP 必须结构化、可预测且无歧义。

---

## 3. OLAMIP 文件（`/olamip.json`）

OLAMIP 文件是一个结构化 JSON 文档，托管在：

`https://yourdomain.com/olamip.json`

它包含：

- `protocol` — 必须是 `"OLAMIP"`  
- `version` — 协议版本  
- `identity` — 你的身份信息  
- `content` — 网站的结构化层级  
- `metadata` — 语言、最后更新时间以及其他全局字段  

### 3.1 发现标签

为了确保 AI 系统能够可靠地定位你的 OLAMIP 文件，请同时包含以下两项：

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

同时使用两者可提供冗余、兼容性和未来适配能力。

---

## 4. 文件结构

### 4.1 Identity 对象

用于描述网站或组织：

- `name` — 必填  
- `type` — 必填  
- `canonical_description` — 必填  
- `tags` — 可选  

### 4.2 Content 对象

包含：

- 一个 `overview`  
- `sections`  
- 可选的 `subsections`  
- `entries`（最细粒度的内容单元）  

这支持无限层级嵌套。

### 4.3 Sections

Section 用于组织相关内容。必填字段包括：

- `title`  
- `summary`  
- `url`  
- `section_type`  

可选字段包括：

- `policy`（`"allow"` 或 `"forbid"`）  
- `tags`  
- `priority`  
- `published`  
- `language`  
- `subsections`  
- `entries`  

**Policy 继承：**  
如果省略，policy 将从祖先节点继承。默认值为 `"allow"`。

### 4.4 Entries

Entry 表示单个内容项，例如：

- 博客文章  
- 新闻报道  
- 产品  
- 文档页面  
- 研究论文  
- 媒体项目  

必填字段：

- `title`  
- `summary`  
- `url`  
- `content_type`  

可选字段：

- `policy`、`tags`、`priority`、`published`、`language`、`metadata`  

URL 是必需的，因为它们能唯一标识内容，并允许 AI 系统验证、去重和交叉引用页面。

---

## 5. 元数据与语言支持

metadata 对象可以包含：

```json
{
  "last_updated": "2026-01-21",
  "language": "en",
  "source_url": "https://www.yourwebsite.com/",
  "copyright": "© 2026 Example"
}
```

### 多语言支持

语言可以在以下层级定义：

- 文件级  
- Section 级  
- Entry 级  

使用 BCP‑47 语言代码（例如 `en`、`es`、`pt-BR`、`zh-CN`）。

这有助于 AI 系统：

- 选择正确的分词器  
- 避免语言混杂  
- 提高检索准确率  
- 减少幻觉  

---

## 6. 标签与优先级

### 6.1 标签

标签提供轻量级语义线索。它们必须是：

- 小写  
- 单词形式  
- ASCII  
- 多词概念使用连字符  
- 在所有条目中保持一致  

示例：

| Concept | Valid Tag |
|---------|-----------|
| Los Angeles | `los-angeles` |
| Machine Learning | `machine-learning` |
| Data Science | `data-science` |

标签有助于 AI 系统更准确地聚类、消歧和检索内容。

### 6.2 Priority

取值：

- `high` — 旗舰内容  
- `medium` — 默认值  
- `low` — 小众或过时内容  

请谨慎使用 `high`（条目总数的 5–10%）。

---

## 7. 增量更新（`olamip-delta.json`）

网站会不断演进。OLAMIP 通过一个可选的配套文件支持增量更新：

`/olamip-delta.json`

该文件只包含：

- 新增条目  
- 更新条目  
- 删除的 URL  

### 7.1 为什么增量更新很重要

- AI 系统无需重新处理整个文件即可保持最新。  
- 大型目录仍然易于管理。  
- 更新传播更快。  
- 不会遗漏变更。  

### 7.2 滚动增量窗口（推荐）

将过去 7–30 天内的所有 delta 保存在同一个文件中。  
AI 系统会按时间顺序应用它们，以保持同步。

### 7.3 版本化增量文件（替代方案）

发布多个文件：

`/olamip-delta-2026-03-01.json`  
`/olamip-delta-2026-03-02.json`  
...  

适用于高流量站点（新闻、电商、文档）。

### 7.4 发现

只有主 OLAMIP 文件需要 `<link>` 和 `<meta>` 标签。  
AI 系统会自动在同一目录中查找 delta 文件。

---

## 8. OLAMIP 与传统站点地图

- XML Sitemap： “这是我的页面。”  
- OLAMIP： “这是我网站的含义。”  

Sitemap 只是列出 URL。  
OLAMIP 提供：

- 摘要  
- 内容类型  
- 章节层级  
- 标签  
- 元数据  
- 优先级  
- 摄取策略  

结合 schema.org，OLAMIP 能为 AI 系统提供对你网站完整、人工整理过的理解。

---

## 9. 常见问题（摘要）

### OLAMIP 会暴露隐私信息吗？

不会。它只会总结公开可用的内容。

### 竞争对手可以使用我的 OLAMIP 文件吗？

不会超过他们已经使用你的公开页面的程度。

### OLAMIP 文件可以有多大？

通常为 1–10 MB。  
即使有 3,000+ 条条目，也属于正常且安全的范围。

### 我应该多久更新一次？

每当内容发生变化时就更新——把它当作面向 AI 的站点地图。

### 如果我使用 delta，还需要更新主文件吗？

需要。`olamip.json` 始终是权威快照。

### LLM 真的会读取它吗？

会——这正是协议的目的。  
你可以用以下方式测试：

```bash
curl -A "GPTBot" https://yourdomain.com/olamip.json
curl -A "ClaudeBot" https://yourdomain.com/olamip.json
curl -A "PerplexityBot" https://yourdomain.com/olamip.json
```

---

## 10. 总结

OLAMIP 将你的网站转变为一张机器可解释的知识地图：

- `olamip.json` — 完整、结构化的快照  
- `olamip-delta.json` — 增量变更日志  
- Sections、subsections、entries — 清晰的层级结构  
- 摘要、标签、优先级 — 人工整理的意义  
- 发现标签 — 可靠的摄取机制  
- 多语言支持 — 面向全球的准备  

通过采用 OLAMIP，你可以确保 AI 系统按照你的意图理解你的内容——准确、一致，并可规模化。
