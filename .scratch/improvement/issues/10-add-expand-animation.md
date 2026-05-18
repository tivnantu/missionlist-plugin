Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

添加展开/折叠动画：会话详情展开/折叠时使用 CSS transition 实现平滑动画（`max-height` + `opacity`，0.2 秒）。

**端到端行为**：
- 点击会话项 → 详情区域平滑展开（0.2 秒）
- 再次点击 → 详情区域平滑折叠（0.2 秒）
- 动画使用 CSS transition：`max-height` + `opacity`
- 动画流畅，无闪烁或跳变

**决策来源**：
- PRD User Story #20
- "less is more" 原则：简单动画提升质感，不增加复杂度

## Acceptance criteria

- [ ] 展开时使用 `max-height` + `opacity` 动画（0.2 秒）
- [ ] 折叠时使用相同动画
- [ ] 动画流畅，无闪烁或跳变
- [ ] 动画使用 CSS transition 实现

## Blocked by

None - can start immediately
