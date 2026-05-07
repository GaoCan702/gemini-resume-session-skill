---
name: resume-session
description: Resume an interrupted task or session from another AI coding agent (like Cursor, Cline, Windsurf, Aider, or Gemini CLI) by finding, reading, and parsing their local history logs.
---
# Resume Session Skill

This skill allows Gemini CLI to seamlessly resume a task from a previously interrupted session of another AI coding agent. When tokens run out or a session breaks, the user can use this skill to point Gemini CLI to the previous agent's logs, understand the context, and continue the work.

## Quick Start

1. **Identify the previous agent and context:** If the user hasn't specified, ask them which agent they were previously using (e.g., Cursor, Cline, Windsurf, Aider, Gemini CLI) and any identifying info about the task or thread (e.g., "the task about fixing the login button").
2. **Find the logs:** Refer to [locations.md](references/locations.md) to locate the storage path for the specified agent.
3. **Parse the history:** 
   - For JSON/Markdown logs (Cline, Aider, Gemini CLI), use standard tools to read the files. Use `ls -lt` to find the most recently modified files.
   - For SQLite logs (Cursor), use the `sqlite3` shell command.
   - For Protocol Buffers (Windsurf), suggest the user export the logs or attempt string extraction via the `strings` utility.
4. **Summarize and confirm:** Read the tail of the log to determine what was accomplished and what the next steps were. Present a brief summary of the state to the user and confirm if this matches the task they want to resume.
5. **Resume the task:** Once confirmed, adopt the context and continue the task as normal.

## Guidelines

- **Be conservative with context window:** Agent logs can be extremely long. When parsing JSON or SQLite databases, use `tail`, `grep`, or SQL `LIMIT` clauses to fetch only the most recent messages (e.g., the last 5-10 exchanges).
- **Graceful degradation:** If you cannot parse an agent's proprietary format (like Windsurf's `.pb` files), instruct the user to manually export the chat history from their IDE and paste it into the project folder.
- **Maintain project context:** When resuming a task, also remember to check the project's current git status (`git status` and `git diff`) to understand what files were modified right before the token limit was hit. The log tells you the intent; the Git status tells you the reality.