# ADR-0004: 撤销/重做持久化策略

日期: 2026-05-18
状态: 已接受

## 上下文

需要为删除、重命名、排序操作添加撤销/重做功能（Ctrl+Z / Ctrl+Y）。

持久化选项：
- **方案 A**：持久化到磁盘（`undo-stack.json`）
- **方案 B**：仅内存栈（不持久化）
- **方案 C**：混合模式（内存 + 定期持久化）

## 决策

**采用方案 B：仅内存栈（不持久化到磁盘）**

## 后果

**优点**：
- 实现简单（无需文件 IO）
- 无持久化文件管理问题
- 符合"less is more"原则

**缺点**：
- 扩展重新加载后撤销栈丢失
- 仅支持单次会话内的撤销/重做

**设计细节**：
- 撤销栈最大 10 层（环形缓冲）
- 支持操作：删除、重命名、排序
- 删除撤销需要恢复目录 + 标题文件

## 替代方案

- 方案 A（持久化）：增加复杂度，撤销栈跨会话的意义不大
- 方案 C（混合）：过度设计，当前阶段不需要

## 撤销栈数据结构

```typescript
interface UndoAction {
    type: 'delete' | 'rename' | 'reorder';
    timestamp: number;
    data: any; // 操作相关的恢复数据
}

// 在 main.js 中
let undoStack: UndoAction[] = [];
let redoStack: UndoAction[] = [];
const MAX_UNDO_DEPTH = 10;
```
