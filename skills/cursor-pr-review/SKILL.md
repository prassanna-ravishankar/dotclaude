---
name: cursor-pr-review
description: Review PR or branch changes with Cursor CLI using a different model (gemini-3-pro).
---

# Cursor PR Review

Second opinion on code changes via Cursor CLI.

## Setup

Confirm with user:
- **Model**: `gemini-3-pro` / `gpt-4o` / `claude-sonnet`
- **Scope**: `all` / `security` / `bugs` / `performance` / `style`
- **Target**: PR number or "this branch"

## Hands-off

Triggers: "AFK", "review and fix", "handle it".

1. Run Cursor
2. Fix all issues found
3. Report summary

## Hands-on

Default mode.

1. Show instruction to user for approval/edits
2. Run Cursor
3. Present findings
4. User selects issues to fix
5. Fix together

## Run Cursor

```bash
agent -p --model "<MODEL>" --output-format text \
  "Review <TARGET> for <SCOPE> issues. Report each with file:line, severity, description, and fix."
```
