Status: ready-for-agent

## Parent

.scratch/improvement/PRD.md

## What to build

添加撤销/重做功能：`Ctrl+Z` 撤销最后一次操作（删除/重命名/排序），`Ctrl+Y` 重做已撤销的操作。使用内存栈（最大 10 层），不持久化到磁盘。

**端到端行为**：
- 删除会话后按 `Ctrl+Z` → 会话恢复（目录重建 + 标题恢复）
- 重命名会话后按 `Ctrl+Z` → 恢复原名
- 拖拽排序后按 `Ctrl+Z` → 恢复原顺序
- 按 `Ctrl+Y` → 重做上次撤销的操作
- 栈满 10 层后，最早的记录被丢弃（环形缓冲）

**决策来源**：
- PRD Decision 10（撤销/重做实现：内存栈，最大 10 层）
- User Stories #13, #14

## Acceptance criteria

- [ ] `Ctrl+Z` 撤销最后一次操作（删除/重命名/排序）
- [ ] `Ctrl+Y` 重做上次撤销的操作
- [ ] 撤销栈最大 10 层（环形缓冲）
- [ ] 删除撤销后恢复会话（目录 + 标题文件）
- [ ] 重命名撤销后恢复原名
- [ ] 排序撤销后恢复原顺序
- [ ] 撤销/重做不持久化（仅内存）

## Blocked by

None - can start immediately
