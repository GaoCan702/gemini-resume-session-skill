# Storage Locations of Popular AI Coding Agents

When a user asks you to resume a session from another agent, use the paths below to locate the chat history.

## 1. Cline (formerly Claude Dev) / Roo Code (VS Code Extension)
- **Format**: JSON
- **Paths**:
  - macOS: `~/Library/Application Support/Code/User/globalStorage/saoudrizwan.claude-dev/tasks/`
  - Windows: `%APPDATA%\Code\User\globalStorage\saoudrizwan.claude-dev\tasks\`
  - Linux: `~/.config/Code/User/globalStorage/saoudrizwan.claude-dev/tasks/`
- **Details**: Inside `tasks/`, each folder is a task ID. Look for `api_conversation_history.json` or `ui_messages.json`. The file `state/taskHistory.json` is an index of all tasks.
- **How to parse**: Use standard file reading or `jq` to parse JSON.

## 2. Cursor
- **Format**: SQLite (`state.vscdb`) and JSONL
- **Paths**:
  - macOS: `~/Library/Application Support/Cursor/User/`
  - Windows: `%APPDATA%\Cursor\User\`
  - Linux: `~/.config/Cursor/User/`
- **Global Storage (Main Messages)**: `User/globalStorage/state.vscdb` (table `cursorDiskKV` where key is `bubbleId:%` or `composerData:%`).
- **Workspace Storage (Metadata)**: `User/workspaceStorage/<hash>/state.vscdb`.
- **Agent Transcripts (Newer Versions)**: `~/.cursor/projects/<project-id>/agent-transcripts/` (JSONL).
- **How to parse**: Use `sqlite3` CLI to query `state.vscdb`. Example:
  `sqlite3 ~/.config/Cursor/User/globalStorage/state.vscdb "SELECT value FROM cursorDiskKV WHERE key LIKE 'bubbleId:%' LIMIT 10;"`

## 3. Gemini CLI
- **Format**: JSON
- **Paths**: 
  - `~/.gemini/tmp/<project_hash>/chats/`
- **Details**: Inside this directory, the chat threads are stored as JSON files. You can find the specific `<project_hash>` by looking at directories inside `~/.gemini/tmp/`.
- **How to parse**: Standard file reading or `jq`.

## 4. Windsurf (Cascade)
- **Format**: Protocol Buffers (`.pb`)
- **Paths**:
  - macOS / Linux: `~/.codeium/windsurf/cascade/`
  - Windows: `C:\Users\<Username>\.codeium\windsurf\cascade\`
- **How to parse**: These are binary files. It's difficult to parse them perfectly without protoc definitions. Suggest the user to export the chat from Windsurf's UI, or try `strings` command as a fallback to extract raw text (e.g. `strings ~/.codeium/windsurf/cascade/*.pb | tail -n 100`).

## 5. Aider
- **Format**: Markdown
- **Paths**: `.aider.chat.history.md` (Usually in the project root directory).
- **How to parse**: Standard file reading.
