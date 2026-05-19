# ADR-0002: SQL 注入修复方式

日期: 2026-05-18
状态: 已接受

## 上下文

`historyReader.ts` 中的 `renameChatHistory()` 函数使用字符串拼接构建 SQL 查询：

```typescript
const ok = await execSqlite(
    dbPath,
    `UPDATE sessions SET custom_title = '${escapedTitle}' WHERE REPLACE(id, '-', '') = '${sessionDir}'`
);
```

存在 SQL 注入风险（尽管已做简单的单引号转义）。

修复选项：
- **方案 A**：改进转义函数（不可靠，容易遗漏边界情况）
- **方案 B**：使用参数化查询（sql.js 支持）
- **方案 C**：切换到其他数据库库（如 `better-sqlite3`，但增加依赖）

## 决策

**采用方案 B：使用参数化查询**

修改 `execSqlite()` 函数签名，支持参数化查询：

```typescript
// 修改前
export function execSqlite(dbPath: string, query: string): boolean

// 修改后
export function execSqlite(dbPath: string, query: string, params?: any[]): boolean
```

调用方式改为：

```typescript
const ok = await execSqlite(
    dbPath,
    `UPDATE sessions SET custom_title = ? WHERE REPLACE(id, '-', '') = ?`,
    [newTitle, sessionDir]
);
```

## 后果

**优点**：
- 彻底消除 SQL 注入风险
- sql.js 原生支持参数化查询
- 代码更清晰（SQL 和参数分离）

**缺点**：
- 需要修改所有 `historyReader.ts` 中的 SQL 查询调用
- 需要修改 `execSqlite()` 和 `querySqlite()` 的函数签名

**风险**：
- 修改范围较大，需要仔细测试所有 SQL 操作

## 替代方案

- 方案 A（改进转义）：不可靠，SQL 注入防护应该依赖参数化而不是转义
- 方案 C（切换库）：增加依赖，sql.js 已经能满足需求
