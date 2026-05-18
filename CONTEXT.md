# CodeBuddy History Viewer - Domain Context

## Overview

A VS Code / CodeBuddy IDE extension that displays CodeBuddy chat history in the sidebar. Users can browse, search, reorder, and jump back to previous conversations.

## Core Concepts

### Session (会话)

A single conversation with CodeBuddy. Identified by:
- `sessionDir` — directory name (UUID format), also the `conversationId` in CodeBuddy IDE
- `sessionId` — database UUID (with hyphens), used for deep link navigation
- `workspaceHash` — MD5 hash of the workspace path, used to isolate sessions per project

### Workspace Hash

Computed as `MD5(path.normalize(workspacePath))`. Used to locate the correct history directory under `CodeBuddyExtension/Data/<account>/CodeBuddyIDE/<uid>/history/<workspaceHash>/`.

Windows compatibility: the plugin tries both upper-case and lower-case drive letters when matching.

### Status (状态)

Sessions have a computed status with priority (high to low):
1. `running` — CodeBuddy is processing a request
2. `error` — something went wrong
3. `pending` — waiting in queue
4. `active` — the session the user last clicked (manually declared, 5-min window)
5. `completed` — finished normally
6. `idle` — no recent activity

Status is determined by `statusMonitor.ts` using a multi-source strategy:
- Primary: `requests[].state === 'running'` in `index.json`
- Secondary: manually declared session via `setManualActiveSession()`
- Tertiary: `mtime` fallback (most recently modified within 30 min)

### History Reader (数据访问层)

Module (`historyReader.ts`) that:
- Scans `CodeBuddyExtension` data directories
- Reads `index.json` (session list) and `messages/<id>.json` (individual messages)
- Queries `workbuddy.db` (SQLite) for session titles and `cwd` mappings
- Handles rename (`title.txt`), delete (directory removal), and custom order persistence

### Webview (前端视图)

HTML/CSS/JS rendered inside VS Code's sidebar. Communicates with the extension backend via `postMessage`. Key responsibilities:
- Render session list with status indicators
- Handle user interactions (click, drag, rename, delete, expand)
- Request session details and display them inline

## Architecture

```
extension.ts          ← Entry point (activate, register commands)
sidebarProvider.ts    ← Controller (WebviewViewProvider, message routing)
historyReader.ts      ← Service layer (data access, file system, SQLite)
db.ts                 ← Data access layer (sql.js wrapper)
statusMonitor.ts      ← Background process (3-second poll, status computation)
webview/
  main.js             ← Frontend logic (rendering, interaction)
  style.css           ← Frontend styles (VS Code theme variables)
```

## Data Flow

```
User clicks session
    ↓ main.js
vscode.postMessage({type:'openSessionInCodeBuddy'})
    ↓ sidebarProvider.ts
openSessionInCodeBuddy()
    → Try extension API (setCurrentConversation)
    → Try known commands (fallback)
    → Show manual instruction (final fallback)
    ↓
setManualActiveSession() → statusMonitor updates indicators
```

## File Locations

### CodeBuddy Data Directory
```
%LOCALAPPDATA%/CodeBuddyExtension/Data/
└── <account>/
    └── CodeBuddyIDE/
        └── <uid>/
            └── history/
                └── <workspaceHash>/
                    ├── index.json          ← session list
                    └── <sessionDir>/
                        ├── index.json      ← messages + requests + state
                        ├── messages/<id>.json
                        └── title.txt       ← custom title (written by plugin)
```

### Plugin State
```
~/.codebuddy/history-order.json  ← custom drag order (per workspaceHash)
```

### SQLite Database
```
~/.workbuddy/workbuddy.db  ← sessions table (id, title, custom_title, cwd)
```

## Key Terms

| Term | Meaning |
|------|---------|
| `conversationId` | Same as `sessionDir` (UUID without hyphens) |
| `workspaceHash` | MD5 of workspace path, isolates sessions per project |
| `accountId` | CodeBuddy account directory name |
| `uid` | User ID within CodeBuddy IDE |
| `custom_title` | Title stored in `title.txt` (user-defined, overrides DB title) |
| `mtime` | File modification time, used as fallback for active session detection |
| `manualActiveSession` | Session user last clicked, kept for 5 minutes |

## UI/UX Terms

| Term | Meaning |
|------|---------|
| `搜索框` (Search Box) | Real-time title filtering, placed inline with sort dropdown |
| `键盘快捷键` (Keyboard Shortcuts) | Global shortcuts: `Enter` to open, `F2` to rename, `Delete` to delete, `Esc` to clear search |
| `右键菜单` (Context Menu) | Custom DIV overlay (not native), appears at cursor position |
| `收藏/星标` (Favorite/Star) | Sessions pinned at top of list (max 3-5 items) |
| `导出为 Markdown` (Export to Markdown) | Full info export: title + time + status + all messages + timestamps |
| `展开/折叠动画` (Expand/Collapse Animation) | CSS transition: `max-height` + `opacity` (0.2s) |

## Conventions

- Session titles: prefer `title.txt` (file) > `custom_title` (DB) > `title` (DB) > first user message
- Timestamps: prefer `requests[].startedAt` > message file `mtime`
- Sorting: `custom` order stored in `history-order.json`, other sorts computed on-the-fly
- Status: computed fresh every 3 seconds by `statusMonitor`, pushed to webview via `postMessage`

## Skills That Read This File

- `improve-codebase-architecture` — understands domain before suggesting refactors
- `diagnose` — maps error reports to domain concepts
- `tdd` — uses glossary terms in test names and assertions
- `to-prd` — writes PRD using domain vocabulary
