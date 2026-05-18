Status: ready-for-agent

## Parent

.scratch/improvement\PRD.md

## What to build

添加 Marketplace 元数据：在 `package.json` 中添加 `license`、`keywords`、`icon` 字段，使插件在 VS Code Marketplace 上可发现和专业化。

**端到端行为**：
- `package.json` 包含 `license` 字段（如 `"license": "MIT"`）
- `package.json` 包含 `keywords` 字段（如 `["codebuddy", "chat", "history", "AI"]`）
- `package.json` 包含 `icon` 字段（指向 `resources/icon.png`）
- 插件在 Marketplace 上显示图标和关键词

**决策来源**：
- PRD User Story #24
- Phase 3 (v1.0.0) 发布准备

## Acceptance criteria

- [ ] `package.json` 添加 `license` 字段
- [ ] `package.json` 添加 `keywords` 字段（3-5 个关键词）
- [ ] `package.json` 添加 `icon` 字段（指向 `resources/icon.png`）
- [ ] 插件在 Marketplace 上显示图标和关键词

## Blocked by

None - can start immediately
