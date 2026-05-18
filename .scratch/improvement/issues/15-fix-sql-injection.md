Status: ready-for-agent

## Parent

.scratch\improvement\PRD.md

## What to build

修复 SQL 注入漏洞：将 `historyReader.ts` 中的字符串拼接 SQL 查询改为参数化查询。`sql.js` 支持参数化查询。

**端到端行为**：
- `renameChatHistory()` 使用参数化查询（不是字符串拼接）
- `execSqlite()` 支持参数（修改函数签名）
- 所有 `historyReader.ts` 中的 SQL 查询都使用参数化
- 无 SQL 注入风险

**决策来源**：
- PRD Decision 8（SQL 注入修复：参数化查询）
- PRD User Story #25
- Phase 4 (v1.1.0) 安全修复

## Acceptance criteria

- [ ] `renameChatHistory()` 使用参数化查询
- [ ] `execSqlite()` 支持参数（修改函数签名）
- [ ] 所有 `historyReader.ts` 中的 SQL 查询都使用参数化
- [ ] 无 SQL 注入风险（通过静态分析或测试验证）
- [ ] 功能不变（重命名仍然正常工作）

## Blocked by

None - can start immediately
