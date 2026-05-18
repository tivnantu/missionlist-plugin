Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

添加导出到 Markdown 功能：将会话完整信息（标题 + 时间 + 状态 + 所有消息 + 时间戳）导出为 Markdown 文件，保存到用户 Downloads 文件夹。

**端到端行为**：
- 右键菜单选择"导出到 Markdown" → 生成 Markdown 文件
- 文件内容：标题 + 时间 + 状态 + 所有消息 + 时间戳
- 文件保存到用户 Downloads 文件夹
- 导出完成后显示通知（"已导出到：~/Downloads/xxx.md"）

**决策来源**：
- PRD Decision 7（导出 Markdown 格式：完整信息导出）
- User Story #19

## Acceptance criteria

- [ ] 右键菜单包含"导出到 Markdown"选项
- [ ] 导出内容包括：标题 + 时间 + 状态 + 所有消息 + 时间戳
- [ ] 文件保存到用户 Downloads 文件夹
- [ ] 导出完成后显示通知
- [ ] Markdown 格式清晰易读

## Blocked by

#7 (右键菜单完成后，才能添加导出选项)
