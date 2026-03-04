---
name: opencode
description: "Delegate ALL coding tasks to OpenCode, a dedicated AI coding agent. Use when you need to write code, debug, refactor, scaffold projects, review code, run tests, or modify source files. Do not write code directly — delegate to OpenCode via its CLI. Triggers on any request involving code generation, file creation, debugging, refactoring, code review, test execution, or project scaffolding."
---

# OpenCode Coding Agent

You have access to OpenCode, a dedicated AI coding agent.
Use OpenCode for ALL coding tasks: writing code, debugging, refactoring, file creation,
project scaffolding, and code review. Do not write code directly — delegate to OpenCode.

OpenCode runs on a separate model optimized for coding. It has its own tools, file access,
and persistent sessions. You interact with it via CLI commands through your shell.

Run all commands from the target project's directory. Sessions are stored per-project,
so the working directory determines which project OpenCode operates on.

## Quick Reference

| Task | Command |
|------|---------|
| New coding task | `opencode run "task description"` |
| Continue last session | `opencode run --continue "follow-up"` |
| Continue specific session | `opencode run --session SESSION_ID "follow-up"` |
| List sessions | `opencode session list` |
| Check session cost/tokens | `opencode stats` |

## When to Use OpenCode

**Always use OpenCode for:**
- Writing new code files or modules
- Debugging errors or fixing bugs
- Refactoring or restructuring code
- Code review and analysis
- Project scaffolding and boilerplate
- Running tests and interpreting results
- Any task that involves reading or modifying source files

**Do NOT use OpenCode for:**
- Answering general knowledge questions
- Conversational responses
- Tasks that don't involve code or files
- Summarizing non-code documents

## CLI Commands

### Start a new coding task

```bash
opencode run "Implement a REST API endpoint for user authentication"
```

Creates a new session and runs the prompt. All permissions are auto-approved in non-interactive mode.

### Give the session a title

```bash
opencode run --title "User Auth API" "Scaffold a FastAPI project with JWT auth"
```

Makes it easier to find sessions later.

### Continue the most recent session

```bash
opencode run --continue "Now add input validation"
```

Picks up the last session and continues where it left off. Use this for multi-step work to keep context in a single session.

### Continue a specific session

```bash
opencode run --session ses_abc123 "Fix the failing test"
```

Resumes a specific session by ID. Use `session list` to find the right ID.

### Fork a session

```bash
opencode run --session ses_abc123 --fork "Try an alternative approach"
```

Creates a copy of the session and continues from there. The original session is untouched. Useful for exploring alternatives without losing prior work.

### Use a specific model

```bash
opencode run --model anthropic/claude-opus-4-6 "Complex refactor task"
```

Overrides the default model for this run.

### Use a specific agent

```bash
opencode run --agent coder "Write the implementation"
```

Uses a named agent (configured in opencode.json).

### Attach files to a prompt

```bash
opencode run --file error_log.txt "Debug this error"
```

Attaches one or more files as context for the task.

### Get JSON output

```bash
opencode run --format json "List all TODO comments in the codebase"
```

Returns structured JSON instead of formatted text. Useful for parsing results programmatically.

### Quiet mode

```bash
opencode run -q "Quick task"
```

Suppresses the spinner animation for cleaner output in scripts.

### List all sessions

```bash
opencode session list
```

Shows all sessions with IDs, titles, and last updated timestamps.

### List recent sessions

```bash
opencode session list -n 5
```

Shows only the 5 most recent sessions.

### Get session list as JSON

```bash
opencode session list --format json
```

Machine-readable session list.

### View token usage and cost

```bash
opencode stats
```

Shows token usage and cost across all sessions.

### View stats for a time window

```bash
opencode stats --days 7
```

### Export a session

```bash
opencode export ses_abc123
```

Exports full session data as JSON.

### List available models

```bash
opencode models
```

Shows all models available from configured providers.

### List models for a specific provider

```bash
opencode models anthropic
```

## Workflow Patterns

### Pattern 1: Simple one-shot task

Best for small, self-contained tasks.

```bash
opencode run "Create a Python script that reads CSV and outputs JSON"
```

### Pattern 2: Multi-step project work

Use `--continue` to keep context in a single session across multiple steps.

```bash
# Step 1: Start the project
opencode run --title "User Auth API" \
  "Scaffold a FastAPI project with user authentication using JWT tokens"

# Step 2: Check which session was created
opencode session list -n 1 --format json

# Step 3: Continue in the same session
opencode run --continue \
  "Add input validation with Pydantic models and write tests"

# Step 4: Continue further
opencode run --continue \
  "Fix any failing tests and add error handling"
```

### Pattern 3: Explore alternatives

Fork a session to try a different approach without modifying the original.

```bash
# Original approach
opencode run --session ses_abc123 \
  "Implement caching with Redis"

# Fork and try a different approach
opencode run --session ses_abc123 --fork \
  "Implement caching with in-memory LRU cache instead"
```

## Important Notes

- **Run from the project directory.** OpenCode operates on the current working directory. Always `cd` to the target project before running commands.
- **Sessions persist per-project.** Every interaction is saved and tied to the project directory. You can review, continue, or fork any session later.
- **Do not write code directly.** Always delegate to OpenCode via the commands above.
- **Use `--continue` for multi-step work.** This keeps context in a single session rather than creating many disconnected ones.
- **Use `--title` for new tasks.** Makes sessions easier to find later.
- **Report back.** After OpenCode finishes, summarize what was done to the user. Include the session ID so they can review.
- **Verify results.** If a coding task might fail, follow up with a `--continue` to verify or run tests.
