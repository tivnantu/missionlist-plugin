Status: ready-for-agent

## Parent

.scratch\improvement\PRD.md

## What to build

添加核心模块单元测试：`db.ts`、`historyReader.ts`、`statusMonitor.ts`。使用 VS Code 内置测试运行器 (`@vscode/test-electron`) 和 `assert`（Node 内置）。

**端到端行为**：
- `test/db.test.ts` - 测试 `db.ts`（参数化查询、SQL 注入防护）
- `test/historyReader.test.ts` - 测试 `historyReader.ts`（读取、重命名、删除、排序）
- `test/statusMonitor.test.ts` - 测试 `statusMonitor.ts`（状态计算、mtime 缓存）
- 所有测试通过（100% 通过率）
- 测试覆盖核心逻辑（> 80% 覆盖率）

**决策来源**：
- PRD Testing Decisions 部分
- PRD User Story #26
- Phase 4 (v1.1.0) 稳定性保障

## Acceptance criteria

- [ ] `test/db.test.ts` - 测试 `db.ts`
- [ ] `test/historyReader.test.ts` - 测试 `historyReader.ts`
- [ ] `test/statusMonitor.test.ts` - 测试 `statusMonitor.ts`
- [ ] 所有测试通过（100% 通过率）
- [ ] 测试覆盖核心逻辑（> 80% 覆盖率）
- [ ] 使用 `@vscode/test-electron` 和 `assert`

## Blocked by

#15 (SQL 注入修复完成后，因为 `execSqlite()` 签名会变化)
