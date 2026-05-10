# OLAMIP-DELTA 文件格式规范

*版权所有 © 2025 Ralph Gonzalez – https://olamip.org*

OLAMIP-DELTA 协议定义了一种基于 JSON 的增量更新文件格式，用于对 `olamip.json` 进行补充更新。本规范面向熟悉结构化数据、HTML 和站点地图（sitemap）式发布的网站管理员及实现者。

## 1. 概述

`olamip.json` 是您网站结构和内容的权威、全量快照。`olamip-delta.json` 是一个可选的配套文件，用于描述自上次更新以来该快照发生的变更。

两者结合，形成了一个机器可读的变更流：

- `olamip.json`：完整的结构化快照。
- `olamip-delta.json`：增量变更日志。

---

## 2. 核心概念

- **Entry（条目）** — 叶子级内容项（如：博客文章、产品页面、文档页面）。
- **Section / Subsection（栏目/子栏目）** — 层级分组（如：博客分类、产品系列、项目组）。
- **Operation（操作）** — 分为 `added`（新增）、`updated`（更新）、`removed`（删除）。
- **Delta（增量）** — 包含日期范围的变更集，其中包含上述操作字段。

---

## 3. 文件位置与发现

### 3.1 文件位置

olamip.json 必须托管在域名的根目录下：
https://yourdomain.com/olamip.json

olamip-delta.json 必须托管在同一目录下：
https://yourdomain.com/olamip-delta.json

版本化增量文件（可选）也存放在同一目录：
/olamip.json
/olamip-delta-YYYY-MM-DD.json

### 3.2 发现机制

网站管理员必须在网站的 <head> 中使用以下两个标签声明 olamip.json 的位置：

<link rel="olamip" href="https://yourdomain.com/olamip.json">
<meta name="olamip-location" content="https://yourdomain.com/olamip.json">

---

## 4. 顶级结构

在顶级层面上，olamip-delta.json 是一个包含以下字段的 JSON 对象：

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
          "title": "示例条目",
          "url": "https://example.com/page/",
          "summary": "...",
          "content_type": "blog_article"
        }
      ],
      "updated": [
        {
          "url": "https://example.com/page/",
          "summary": "更新后的摘要"
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

### 4.1 顶级必填字段

- protocol (string, 必填): 必须为 "OLAMIP-DELTA"。
- version (string, 必填): 必须符合文档定义的版本方案（如 "1.0.0"）。
- window_days (integer, 可选): 滚动窗口的天数。
- last_updated (string, 必填): 文件最后修改日期 (ISO-8601 格式)。
- deltas (array, 必填): 包含 date, added, updated 和 removed 的增量对象数组。

---

## 5. 增量对象结构

为了确保机器解释的清晰性与一致性，网站管理员必须在每个增量对象中包含所有三个操作字段（added, updated, removed），即使其中某些字段为空。

---

## 6. 操作类型

### 6.1 added
added 中的项必须是一个完整的对象，字段需符合 OLAMIP 主规范。

### 6.2 updated
更新项必须包含 url，可以是部分对象（仅含变更字段）或完整对象。

### 6.3 removed
删除项是极简的，仅包含身份标识：
{ "url": "https://example.com/page/to-remove/" }

---

## 7. URL 与对象身份
OLAMIP-DELTA 使用规范 URL（Canonical URL）作为主要的身份标识。更改 URL 需要执行“先删除后新增”操作，而不是原地更新 URL。

---

## 8. 滚动增量窗口（推荐）
在此模型中，文件包含过去 window_days 天（通常为 7-30 天）的变更。这确保了即使爬虫延迟，AI 系统也不会遗漏更新。

---

## 9. 版本化增量（替代方案）
网站管理员可以选择按日期存放独立文件：olamip-delta-YYYY-MM-DD.json。AI 系统应自动发现目录中的文件并按时间顺序应用。

---

## 10. 结构性变更
相同的增量规则也适用于栏目（sections）和子栏目（subsections）。当一个栏目被删除时，其所有后代（子栏目和条目）在逻辑上也会被视为已删除。

---

## 11. 符合性要求

### 11.1 必须（MUST）规则
- protocol 必须为 "OLAMIP-DELTA"。
- 每个增量对象必须同时包含 added, updated 和 removed 字段。
- URL 必须是绝对路径且为规范 URL。
- deltas 数组必须按日期升序排列。

---

## 12. 最佳实践
1. 通过 CMS 或发布流程自动生成增量文件。
2. 始终保持 olamip.json 的权威性和准确性。
3. 使用稳定的规范 URL 以避免增量记录中出现不必要的波动。

---

## 13. 相关资源
- OLAMIP 文件格式规范: https://olamip.org/file-format-specification/
- 常见问题解答 (FAQ): https://olamip.org/frequently-asked-questions/
- 为什么 OLAMIP 标准优于 LLMs.txt: https://olamip.org/why-olamip-is-a-superior-standard-to-llms-txt/

---
*本文件作为在生产环境中生成、校验和使用 olamip-delta.json 文件的参考规范。*
