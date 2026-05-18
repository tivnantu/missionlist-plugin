Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

添加键盘导航：`↑` `↓` 方向键在会话列表中导航，`Enter` 打开选中会话。选中项高亮显示（使用 `.selected` CSS 类 + VS Code 主题变量）。支持循环导航（到达末尾后回到开头）。

**端到端行为**：
- 按 `↓` → 选中项下移一项
- 按 `↑` → 选中项上移一项
- 在首项按 `↑` → 选中最后一项（循环）
- 在末项按 `↓` → 选中第一项（循环）
- 按 `Enter` → 打开当前选中项
- 选中项高亮显示（使用 VS Code 主题变量）

**决策来源**：
- PRD Decision 11（键盘导航：方向键 + Enter 打开）
- User Story #12

## Acceptance criteria

- [ ] `↓` 键选中项下移
- [ ] `↑` 键选中项上移
- [ ] 支持循环导航（首项 `↑` → 末项，末项 `↓` → 首项）
- [ ] `Enter` 打开当前选中项
- [ ] 选中项高亮显示（使用 VS Code 主题变量）
- [ ] 导航时自动滚动到可见区域

## Blocked by

None - can start immediately
