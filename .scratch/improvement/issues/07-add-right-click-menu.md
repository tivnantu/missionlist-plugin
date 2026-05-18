Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

添加右键自定义菜单：在光标位置显示自定义 DIV 覆盖层，包含菜单项（打开会话 / 重命名 / 收藏 / 导出到 Markdown / 删除）。使用 VS Code 主题变量实现无缝集成。

**端到端行为**：
- 右键点击会话项 → 在光标位置显示菜单
- 菜单项：打开会话 / 重命名 / 收藏 / 导出到 Markdown / 删除
- 点击菜单项执行对应操作
- 点击菜单外区域或按 `Esc` → 关闭菜单
- 菜单样式使用 VS Code 主题变量（无缝集成）

**决策来源**：
- PRD Decision 5（右键菜单实现：自定义 DIV 覆盖层）
- User Stories #15, #16

## Acceptance criteria

- [ ] 右键点击会话项显示自定义菜单
- [ ] 菜单位置在光标位置
- [ ] 菜单项：打开会话 / 重命名 / 收藏 / 导出到 Markdown / 删除
- [ ] 点击菜单项执行对应操作
- [ ] 点击菜单外区域或按 `Esc` 关闭菜单
- [ ] 菜单样式使用 VS Code 主题变量

## Blocked by

#4 (空状态完成后，确保菜单在空状态时不会错误显示)
