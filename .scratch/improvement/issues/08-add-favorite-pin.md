Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

添加收藏/置顶功能：鼠标悬停时显示星标图标，点击切换收藏状态。收藏的会话置顶显示（最多 3-5 项）。使用独立存储文件 `~/.codebuddy/favorites.json`。

**端到端行为**：
- 鼠标悬停在会话项 → 显示星标图标
- 点击星标 → 切换收藏状态（填充/空心）
- 收藏的会话置顶显示（最多 3-5 项）
- 收藏状态持久化到 `favorites.json`
- 取消收藏后，会话回到原位置

**决策来源**：
- PRD Decision 6（收藏显示：置顶，最多 3-5 项）
- User Stories #17, #18

## Acceptance criteria

- [ ] 鼠标悬停显示星标图标
- [ ] 点击星标切换收藏状态
- [ ] 收藏的会话置顶显示（最多 3-5 项）
- [ ] 收藏状态持久化到 `favorites.json`
- [ ] 取消收藏后会话回到原位置
- [ ] 星标图标使用 VS Code Codicon

## Blocked by

None - can start immediately
