Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

添加搜索框，支持实时按标题过滤会话列表。搜索框放置在排序下拉框同一行（右侧），使用 VS Code Codicon 图标，占位符文本 "搜索会话..."。

**端到端行为**：
- 用户在搜索框输入文字 → 会话列表实时过滤（仅显示标题匹配项）
- 按 `Esc` → 清空搜索框（若已空则失焦）
- 搜索框右侧显示 Codicon `codicon-search`
- 无匹配结果时显示空状态（"未找到匹配的会话"）

**决策来源**：
- PRD Decision 1（搜索框行为：实时标题过滤）
- PRD Decision 2（UI 放置：与排序下拉框同行）
- PRD Decision 3（空状态 UI：Codicon + 引导文本）

## Acceptance criteria

- [ ] 搜索框出现在排序下拉框右侧（同一行）
- [ ] 输入时实时过滤会话列表（仅标题匹配）
- [ ] 占位符显示 "搜索会话..."
- [ ] `Esc` 键清空搜索框（已空则失焦）
- [ ] 搜索框使用 Codicon `codicon-search`（非 emoji）
- [ ] 无匹配结果时显示 "未找到匹配的会话"

## Blocked by

None - can start immediately
