Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

修复 `src/webview/main.js` 第 462-463 行的语法错误。该错误导致会话详情展开时 JavaScript 执行中断，无法显示用户消息列表。

**端到端行为**：
- 点击会话条目 → 展开显示该会话的用户消息列表
- 无 JS 错误，控制台无红色错误
- 消息按时间倒序排列（最新消息在最前）

**决策来源**：
- PRD Decision 8（SQL 注入修复）中发现此 bug
- 实际错误：`extractMessageContent` 函数返回值处理不当

## Acceptance criteria

- [ ] `main.js` 第 462-463 行语法错误已修复
- [ ] 点击会话条目能正常展开/折叠详情
- [ ] 控制台无 JS 错误
- [ ] 会话详情显示该会话的用户消息列表

## Blocked by

None - can start immediately
