# OLAMIP 文件格式规范

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

## 概述

OLAMIP 文件（`/olamip.json`）是一个结构化的 JSON 文档，用于提供网站中最重要页面的精选摘要。该文件专为大型语言模型（LLM）设计，使其能够以清晰、精确且有意图的方式理解、优先处理和使用您的内容。

## 文件位置

OLAMIP 文件必须托管在您的域名根目录：

`https://yourdomain.com/olamip.json`

## 声明 OLAMIP 文件位置

为了确保系统能够可靠地发现您的 OLAMIP 文件，请在网站的 `<head>` 部分同时添加 `<link>` 标签和 `<meta>` 标签。

### 主要发现方式：`<link rel="olamip">`

- 标准化实践：爬虫已会扫描 `<link>` 标签（如 canonical、sitemap 等）。
- 机器友好：建立页面与 OLAMIP 文件的正式关系。
- 互操作性：与现有 Web 标准兼容，便于 AI 系统采用。

### 备用发现方式：`<meta name="olamip-location">`

- 人类可读：简单易懂。
- 兼容性：部分工具更偏好 `<meta>` 标签。
- 冗余性：作为备用机制。

### 推荐实现

```html
<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">
```

## 文件结构

必须为有效 UTF‑8 JSON，并包含：

- `protocol`
- `version`
- `identity`
- `content`
- `metadata`

### 示例结构

```json
{
  "protocol": "OLAMIP",
  "version": "1.0.0",
  "identity": {},
  "content": {},
  "metadata": {}
}
```

## Identity 对象

| 字段 | 类型 | 必填 | 说明 |
|---|---|---:|---|
| `name` | `string` | 是 | 网站或组织名称 |
| `type` | `string` | 是 | 类型（如 company、blog） |
| `canonical_description` | `string` | 是 | 网站描述 |
| `tags` | `array<string>` | 否 | 关键词 |

## Content 对象

包含：

- `overview`
- `sections`

### Overview

| 字段 | 类型 | 必填 | 说明 |
|---|---|---:|---|
| `summary` | `string` | 是 | 网站简要说明 |

## Section 规范

表示内容分类。

### 字段

| 字段 | 类型 | 必填 | 说明 |
|---|---|---:|---|
| `title` | `string` | 是 | 标题 |
| `summary` | `string` | 是 | 描述 |
| `url` | `string` | 是 | URL |
| `section_type` | `string` | 是 | 分类类型 |
| `policy` | `string` | 否 | `"allow"` 或 `"forbid"` |
| `entries` | `array<Entry>` | 是 | 内容列表 |
| `subsections` | `array<Section>` | 否 | 子分类 |

## 策略（Policy）

- `"allow"`：允许
- `"forbid"`：禁止

默认值：`"allow"`

### 继承规则

1. Entry
2. Subsection
3. Section
4. 默认值

## Entry 对象

最小内容单元。

### 字段

| 字段 | 类型 | 必填 | 说明 |
|---|---|---:|---|
| `title` | `string` | 是 | 标题 |
| `summary` | `string` | 是 | 摘要 |
| `url` | `string` | 是 | 绝对 URL |
| `content_type` | `string` | 是 | 内容类型 |

## 内容类型

- `page`
- `blog_article`
- `news_article`
- `product`
- `doc_page`
- `research_paper`

## 数组字段

示例：

```json
"tags": ["ai", "search", "llm"]
```

## Metadata

```json
{
  "last_updated": "2026-01-21",
  "language": "zh-CN"
}
```

## 优先级

- `"high"`
- `"medium"`
- `"low"`

## 多语言支持

使用 BCP‑47：

- `zh-CN`
- `en`
- `ja`

## 验证规则

- JSON 必须有效
- 使用绝对 URL
- 不允许尾随逗号

## 标签（Tags）

规则：

- 小写
- 单词
- 使用连字符

示例：

- `machine-learning`
- `data-science`

## 版本控制

- 忽略未知字段
- 使用最新规范

## 语义对齐

OLAMIP 补充 schema.org、网站地图和爬虫系统，为 AI 提供结构化语义层以提升理解和检索能力。
