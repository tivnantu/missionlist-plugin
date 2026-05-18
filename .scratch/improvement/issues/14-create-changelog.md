Status: ready-for-agent

## Parent

.scratch\improvement\PRD.md

## What to build

创建 CHANGELOG.md，记录插件版本历史和更新内容。使用 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) 格式。

**端到端行为**：
- CHANGELOG.md 包含所有版本历史（从 v0.5.9 开始）
- 每个版本包含：新增功能、修复、变更
- 使用 Keep a Changelog 格式
- 包含未发布版本（Unreleased）部分

**决策来源**：
- PRD User Story #23（隐含：发布需要 CHANGELOG）
- Phase 3 (v1.0.0) 发布准备
- Decision：使用 Keep a Changelog 格式（已在 PRD 中确认）

## Acceptance criteria

- [ ] CHANGELOG.md 包含所有版本历史（从 v0.5.9 开始）
- [ ] 每个版本包含：新增功能、修复、变更
- [ ] 使用 Keep a Changelog 格式
- [ ] 包含 Unreleased 部分
- [ ] CHANGELOG.md 可读性好（Markdown 格式）

## Blocked by

#13 (README 完成后，因为 CHANGELOG 可能引用 README 中的功能描述)
