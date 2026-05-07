# Resume Session Skill for Gemini CLI

A specialized skill for Gemini CLI that enables it to resume tasks from other AI coding agents by reading their local session logs.

## Supported Agents

- **Cline / Roo Code**
- **Cursor**
- **Windsurf (Cascade)**
- **Aider**
- **Gemini CLI**

## How to Install

1. Download the `resume-session.skill` file (if available) or clone this repository.
2. Install the skill:
   ```bash
   gemini skills install path/to/resume-session.skill --scope user
   ```
3. Reload skills in your Gemini CLI session:
   ```bash
   /skills reload
   ```

## Usage

When your session with another agent is interrupted (e.g., ran out of tokens), start Gemini CLI in the same project and say:

> "I was using [Agent Name] for [Task Description]. Please use the `resume-session` skill to find the logs and continue."

Gemini CLI will then:
1. Locate the agent's local logs.
2. Parse the most recent messages.
3. Summarize the progress.
4. Adopt the context and continue the task.
