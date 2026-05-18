---
title: "CodeBuddy History Viewer - v1.0 Improvements"
status: ready-for-agent
---

# CodeBuddy History Viewer - v1.0 Improvements

## Problem Statement

The current CodeBuddy History Viewer plugin (v0.5.9) functions as a basic sidebar viewer for chat history, but lacks several key features and polish expected of a professional VS Code extension:

1. **Search is missing** — users cannot quickly find sessions when the list grows
2. **No keyboard shortcuts** — all operations require mouse interaction
3. **Empty state is pure text** — unprofessional first impression
4. **Right-click menu missing** — standard VS Code extension标配 is absent
5. **No favorite/pin feature** — cannot prioritize frequently-used sessions
6. **No export functionality** — users cannot archive or share conversations
7. **SQL injection risk** — string concatenation in `historyReader.ts`
8. **Marketplace metadata incomplete** — missing license, keywords, icon in `package.json`
9. **No message content search** — title search insufficient for finding specific discussions
10. **No undo/redo** — accidental delete/rename/sort cannot be reverted
11. **No keyboard navigation** — arrow keys don't navigate the session list

The plugin works, but feels incomplete. This PRD scopes the work to reach v1.0 (professional, complete product).

---

## Solution

A phased improvement plan that adds essential features while strictly following "less is more" principles:

**Phase 1 (v0.6.0)**: Fix P0/P1 bugs + add search box + keyboard shortcuts + improve empty state + undo/redo + keyboard navigation
**Phase 2 (v0.7.0)**: Add right-click menu + favorite/pin + export to Markdown + expand/collapse animation + message content search
**Phase 3 (v1.0.0)**: Complete Marketplace metadata + README GIF + CHANGELOG
**Phase 4 (v1.1.0)**: Fix SQL injection + add unit tests for core modules

Features explicitly **out of scope** are listed in the "Out of Scope" section.

---

## User Stories

### Phase 1: Basic Polish

1. As a user, I want to search sessions by title in real-time, so that I can quickly find a specific conversation among many
2. As a user, I want to open a session by pressing `Enter`, so that I can navigate without using the mouse
3. As a user, I want to delete a session by pressing `Delete`, so that keyboard-only operation is possible
4. As a user, I want to rename a session by pressing `F2`, so that all common actions have keyboard shortcuts
5. As a user, I want the search box placed inline with the sort dropdown, so that vertical space is preserved
6. As a user, I want the search box to use VS Code Codicon icons (not emoji), so that the UI looks professional and consistent
7. As a user, I want a placeholder "搜索会话..." in the search box, so that I know what it does
8. As a user, I want pressing `Esc` in the search box to clear the input (or blur if already empty), so that I can quickly reset without using the mouse
9. As a user, I want a confirmation dialog when pressing `Delete`, so that I don't accidentally lose a session
10. As a user, I want `Enter` to immediately switch sessions (no extra confirmation), so that navigation is fast
11. As a user, I want the empty state to show a Codicon + guide text, so that the plugin looks professional even when there's no data
12. As a user, I want to navigate the session list with `↑` `↓` arrow keys, so that I can select sessions without using the mouse
13. As a user, I want `Ctrl+Z` to undo the last action (delete/rename/reorder), so that I can recover from accidental operations
14. As a user, I want `Ctrl+Y` to redo the last undone action, so that I can restore an undone operation

### Phase 2: Interaction Enhancement

15. As a user, I want a right-click menu (custom overlay at cursor position), so that I can access all actions without hovering
16. As a user, I want the right-click menu to include: Open Session / Rename / Favorite / Export to Markdown / Delete, so that all common actions are accessible
17. As a user, I want to favorite a session (star icon on hover), so that I can mark sessions I frequently return to
18. As a user, I want favorited sessions pinned to the top of the list (max 3-5), so that my most-used sessions are always accessible
19. As a user, I want to export a session to Markdown (full info: title + time + status + all messages + timestamps), so that I can archive or share conversations
20. As a user, I want the expand/collapse of session details to have a smooth animation (`max-height` + `opacity`), so that the UI feels polished
21. As a user, I want to search not only titles but also message content, so that I can find sessions where I discussed a specific topic

### Phase 3: Marketplace Readiness

22. As a user, I want to see a proper icon in the VS Code sidebar, so that the plugin looks professional
23. As a user, I want to see a GIF demo in the README, so that I can quickly understand what the plugin does
24. As a publisher, I want `license`, `keywords`, and `icon` fields in `package.json`, so that the plugin is discoverable and professional on Marketplace

### Phase 4: Stability & Security

25. As a developer, I want SQL queries to use parameterized queries (not string concatenation), so that there is no SQL injection risk
26. As a developer, I want unit tests covering `historyReader.ts` and `db.ts`, so that core data operations are safe to refactor

---

## Implementation Decisions

### Decision 1: Search box behavior
- **Decision**: Real-time title filtering (not Enter-to-search, not content search)
- **Rationale**: Matches VS Code command palette behavior; content search would require caching message content (complexity not justified)
- **Interface impact**: New `filterHistory(searchTerm)` function in `main.js`; no backend changes

### Decision 2: Search box UI placement
- **Decision**: Inline with sort dropdown (same row)
- **Rationale**: Saves vertical space; matches `todo-tree` plugin style
- **Interface impact**: `style.css` — search box styled as inline-flex element next to `#sortSelect`

### Decision 3: Empty state UI
- **Decision**: Codicon `codicon-comment-discussion` + guide text (no button)
- **Rationale**: "Less is more" — button would add complexity; text guide is enough
- **Interface impact**: `main.js` → `renderEmptyState()`; `style.css` → `.empty-state` + `.empty-icon` classes

### Decision 4: Keyboard shortcuts scope
- **Decision**: Global availability (not just when list item focused)
- **Rationale**: Matches VS Code native behavior; if no item selected, shortcut silently ignored (no error)
- **Keys**: `Enter` = open, `Delete` = delete (with confirmation), `F2` = rename
- **Interface impact**: `main.js` → `document.addEventListener('keydown', ...)` at root level

### Decision 5: Right-click menu implementation
- **Decision**: Custom DIV overlay (not native `contextmenu` / `showQuickPick`)
- **Rationale**: Context menu should appear at cursor position (standard desktop app behavior); custom DIV can use VS Code theme variables for seamless integration
- **Interface impact**: `main.js` → new `showContextMenu(x, y, chatId)` function; `style.css` → `.context-menu` + `.context-menu-item` classes

### Decision 6: Favorite display
- **Decision**: Pinned to top of list (max 3-5 items)
- **Rationale**: User favorites are for quick access; pinning to top matches browser bookmark behavior
- **Implementation**: `sortAndDisplayHistory()` → if `currentSort === 'custom'`, prepend favorited items to `currentHistory` array
- **Storage**: New `favorites.json` in `~/.codebuddy/` (separate from `history-order.json`)

### Decision 7: Export to Markdown format
- **Decision**: Full info export (title + time + status + all messages + timestamps)
- **Rationale**: Export is for archiving/sharing — needs full context; Markdown is lightweight and widely supported
- **Implementation**: New `exportChatToMarkdown(chatDetail)` function in `sidebarProvider.ts`; sends `type: 'exportChat'` message to extension backend; backend writes file to user's Downloads folder

### Decision 8: SQL injection fix
- **Decision**: Parameterized queries in `db.ts` (modify `execSqlite` to support parameters)
- **Rationale**: Eliminates injection risk at the root; `sql.js` supports parameterized queries
- **Implementation**: Change `execSqlite(dbPath, query, params?)` signature; update all callers in `historyReader.ts`

### Decision 9: Message content search
- **Decision**: Lazy-load message content on search (not pre-cache all content)
- **Rationale**: Pre-caching all message content would consume too much memory; lazy-load on search balances performance and resource usage
- **Implementation**: When search term matches title → show immediately; when search term might match content → read `messages/*.json` files on-demand; cache results for current search session
- **Interface impact**: `main.js` → `searchContent(searchTerm)` function; `sidebarProvider.ts` → new `type: 'searchContent'` message handler

### Decision 10: Undo/Redo implementation
- **Decision**: In-memory undo stack (not persisted to disk)
- **Rationale**: Undo is for accidental operations within a session; persisting undo history across sessions adds unnecessary complexity
- **Stack depth**: Max 10 operations (circular buffer)
- **Supported operations**: Delete, Rename, Reorder
- **Storage**: `undoStack: Array<UndoAction>` + `redoStack: Array<UndoAction>` in `main.js`
- **Interface impact**: `main.js` → `pushUndoAction(action)` + `undo()` + `redo()` functions; `Ctrl+Z` / `Ctrl+Y` keydown handlers

### Decision 11: Keyboard navigation (arrow keys)
- **Decision**: Arrow keys navigate list; `Enter` opens selected session
- **Rationale**: Standard list navigation pattern; matches VS Code explorer behavior
- **Visual indicator**: Highlight selected item with `.selected` CSS class (using VS Code theme variables)
- **Wrap-around**: `↓` at last item wraps to first; `↑` at first item wraps to last
- **Interface impact**: `main.js` → `selectedIndex` state variable; `document.addEventListener('keydown', ...)` handlers for `ArrowUp` / `ArrowDown`

---

## Testing Decisions

### What makes a good test
- Test **external behavior** (input → output), not implementation details
- Test **edge cases** (empty history, special characters in titles, Windows paths)
- Test **state transitions** (status changes from `running` → `completed`)

### Modules to be tested (Phase 4)
1. **`db.ts`** — SQLite wrapper
   - Test: Parameterized query execution
   - Test: SQL injection attempts are safely handled
   - Prior art: None in current codebase; use `mocha` + `assert`

2. **`historyReader.ts`** — Data access layer
   - Test: `readCodeBuddyHistory()` returns correct structure
   - Test: `computeWorkspaceHash()` produces consistent hashes
   - Test: `renameChatHistory()` writes to correct location
   - Test: `deleteChatHistory()` removes directory
   - Prior art: None; create `test/historyReader.test.ts`

3. **`statusMonitor.ts`** — Status computation
   - Test: Status priority ordering (running > error > pending > active > completed > idle)
   - Test: mtime cache avoids unnecessary re-reads
   - Prior art: None; create `test/statusMonitor.test.ts`

4. **`main.js`** — Frontend logic (Phase 4)
   - Test: Keyboard navigation (arrow keys) cycles through list correctly
   - Test: Undo/redo stack behaves correctly (push/pop/limit)
   - Test: Message content search returns correct results
   - Prior art: None; create `test/main.test.ts` (Jest)

### Test runner
- Use VS Code's built-in test runner (`@vscode/test-electron`)
- Assertions: `assert` (Node built-in)
- Mock: `fs` calls via `mock-fs` (or test against temp directories)
- Frontend tests: `Jest` + `jsdom`

---

## Out of Scope

The following features were explicitly discussed and **rejected** to maintain "less is more":

1. **Settings UI** — zero-config; defaults are optimal
2. **Grouped display** (Today/This Week/Older) — sort functionality is sufficient; grouping adds visual noise
3. **Batch operations** (multi-select delete/export) — violates simplicity; rare use case
4. **Status bar integration** — activity bar icon is sufficient; avoids UI pollution
5. **Multi-workspace switching** — 99% of users only use current workspace; adds complexity
6. **Tree view** — list view is clear enough; tree adds cognitive load
7. **Advanced search** (regex, complex filters) — simple content search is sufficient for MVP
8. **Cloud sync** — local-first; users can export to share

---

## Further Notes

### Glossary terms used (must match CONTEXT.md)
- `sessionDir` — UUID format, the conversationId in CodeBuddy IDE
- `workspaceHash` — MD5 of workspace path, isolates sessions per project
- `status` — computed value: running/error/pending/active/completed/idle
- `manualActiveSession` — session user last clicked, kept for 5 minutes
- `search box` — real-time title + content filtering, inline with sort dropdown
- `keyboard shortcuts` — global: Enter/F2/Delete/Esc/Ctrl+Z/Ctrl+Y
- `keyboard navigation` — arrow keys (↑↓) to select, Enter to open
- `right-click menu` — custom DIV overlay at cursor position
- `favorite/star` — sessions pinned to top (max 3-5)
- `export to Markdown` — full info export
- `undo/redo` — in-memory stack (max 10), supports delete/rename/reorder

### ADRs needed
1. **ADR-0001**: Custom DIV overlay vs native context menu → decision: custom DIV (matches desktop app behavior, seamless theme integration)
2. **ADR-0002**: Parameterized queries for SQL injection fix → decision: modify `execSqlite()` to accept parameters
3. **ADR-0003**: Message content search approach → decision: lazy-load on search (not pre-cache)
4. **ADR-0004**: Undo/redo persistence → decision: in-memory only (not persisted to disk)

### Release plan
- **v0.6.0**: Phase 1 (search + keyboard + empty state + undo/redo + keyboard navigation)
- **v0.7.0**: Phase 2 (right-click + favorite + export + animation + message content search)
- **v1.0.0**: Phase 3 (Marketplace metadata + README + CHANGELOG)
- **v1.1.0**: Phase 4 (SQL injection fix + unit tests)

---

_Generated by `/to-prd` skill | Updated: 2026-02-16 | Status: ready-for-agent | Do not modify this section_
