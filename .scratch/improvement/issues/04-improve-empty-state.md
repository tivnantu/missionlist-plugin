Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

改进空状态 UI：当无会话时显示 Codicon `codicon-comment-discussion` + 引导文本（"暂无聊天记录，开始新的对话吧"），不添加按钮。

**端到端行为**：
- 无会话时显示 Codicon 图标（大号、淡色）
- 图标下方显示引导文本
- 不显示按钮（"less is more" 原则）
- 空状态区域居中显示

**决策来源**：
- PRD Decision 3（空状态 UI：Codicon + 引导文本，无按钮）
- User Story #11

## Acceptance criteria

- [ ] 无会话时显示 `codicon-comment-discussion` 图标
- [ ] 图标下方显示引导文本 "暂无聊天记录，开始新的对话吧"
- [ ] 不显示任何按钮
- [ ] 空状态区域居中显示
- [ ] 图标使用 VS Code 主题变量（支持深色/浅色主题）

## Blocked by

None - can start immediately
