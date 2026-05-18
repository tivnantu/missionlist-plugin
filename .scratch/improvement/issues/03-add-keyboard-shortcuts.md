Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

添加全局键盘快捷键：`Enter` 打开选中会话，`Delete` 删除（带确认对话框），`F2` 重命名，`Esc` 清空搜索或失焦。

**端到端行为**：
- 按 `Enter` → 立即切换到选中会话（无额外确认）
- 按 `Delete` → 弹出确认对话框（"确定要删除 'XXX' 吗？"），确认后删除
- 按 `F2` → 选中会话进入重命名模式
- 按 `Esc` → 若搜索框有内容则清空，否则失焦
- 若无选中会话，快捷键静默忽略（不报错）

**决策来源**：
- PRD Decision 4（键盘快捷键范围：全局可用）
- User Stories #2, #3, #4, #9, #10

## Acceptance criteria

- [ ] `Enter` 打开选中会话（无确认）
- [ ] `Delete` 弹出确认对话框，确认后删除
- [ ] `F2` 进入重命名模式
- [ ] `Esc` 清空搜索框或失焦
- [ ] 无选中会话时快捷键静默忽略
- [ ] 快捷键全局可用（不仅限于列表项聚焦时）

## Blocked by

None - can start immediately
