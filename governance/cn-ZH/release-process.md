# 发布流程

*Copyright © 2025 Ralph Gonzalez – https://olamip.org*

本文档定义了我用于创建、测试和发布 OLAMIP 规范及相关工具的标准化流程。目前，所有发布都由我独自管理。

## 1. 版本管理

我遵循 [Semantic Versioning 2.0.0](https://semver.org/)：

- **MAJOR** 版本：格式或协议发生不兼容变更时。
- **MINOR** 版本：以向后兼容方式添加新功能或 section 时。
- **PATCH** 版本：修复 bug、修正措辞或澄清规则而不改变行为时。

示例：`1.0.0` → `1.0.1` (patch)、`1.1.0` (minor)、`2.0.0` (major)。

## 2. 分支模型

我使用简单的 Git 工作流：

- `main` — 稳定分支，始终可发布状态。
- 特性或修复分支（如 `spec/delta-formats`、`fix/tag-rules`）— 短生命周期变更分支。
- 发布分支（如 `release/v1.1.0`）— 发布前立即创建。

## 3. 发布前提条件

开始发布前，我确保：

- 本次发布计划的所有变更已合并至 `main`。
- 所有自动化检查（linting、schema 验证、构建脚本）通过。
- 属于本次发布的所有 issue 和笔记已关闭或记录。
- `CHANGELOG.md` 已更新，包含本版本的清晰可读条目。
- 文档保持最新，包括：
  - `file-format-specification.md`
  - `olamip-delta-file-format-specification.md`
  - `docs/introduction.md`
  - `faq.md`
- 示例 `olamip.json` 和 `olamip-delta.json` 文件仍符合最新规则。

## 4. 创建发布候选

1. 从 `main` 创建发布分支：

   ```bash
   git checkout main
   git checkout -b release/vX.Y.Z
   ```

2. 更新版本号（如工具、脚本或标记资产中使用，不仅仅是规范文本）。
3. 更新 `CHANGELOG.md`，添加发布头部和本版本说明。
4. 提交变更：

   ```bash
   git add .
   git commit -m "chore(release): 准备 v1.1.0"
   ```

5. 推送分支：
