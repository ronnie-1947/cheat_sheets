# Claude Code Hooks

Hooks in Claude Code are small scripts that run automatically when specific events happen during a Claude session.

They let you automate workflows like:

* Running tests
* Formatting code
* Reading documentation
* Blocking unsafe commands
* Logging activity
* Sending notifications

Think of hooks as:

“Run this script whenever Claude does X.”

***

## Basic Hook Config

Hooks are configured in a JSON config file.

Example:

```json
{
  "hooks": {
    "post_tool": "./hooks/run-tests.sh"
  }
}
```

This means:

* After Claude uses a tool,
* Run `run-tests.sh`

***

## Common Lifecycle Events

| Event                | When it runs                     |
| -------------------- | -------------------------------- |
| `session_start`      | When Claude Code starts          |
| `user_prompt_submit` | When user submits a prompt       |
| `pre_tool`           | Before a tool runs               |
| `post_tool`          | After a tool runs                |
| `file_change`        | After files are modified         |
| `response_complete`  | After Claude finishes responding |

***

## Example 1 — Run Tests After File Changes

### Config

```json
{
  "hooks": {
    "file_change": "./hooks/test.sh"
  }
}
```

### Script

```bash
#!/usr/bin/env bash

npm test
```

***

## Example 2 — Auto Format Code

### Config

```json
{
  "hooks": {
    "post_tool": "./hooks/format.sh"
  }
}
```

### Script

```bash
#!/usr/bin/env bash

prettier --write .
```

***

## Example 3 — Read README.md for a Component

Useful for giving Claude more context automatically.

### Config

```json
{
  "hooks": {
    "user_prompt_submit": "./hooks/load-readme.sh"
  }
}
```

### Script

```bash
#!/usr/bin/env bash

PROMPT="$CLAUDE_USER_PROMPT"

COMPONENT=$(echo "$PROMPT" | grep -oE 'components/[a-zA-Z0-9_-]+' | head -1)

if [ -n "$COMPONENT" ]; then
  README="$COMPONENT/README.md"

  if [ -f "$README" ]; then
    echo "=== README CONTEXT ==="
    cat "$README"
    echo "=== END README ==="
  fi
fi
```

What this does:

1. User submits prompt
2. Hook runs
3. Finds component folder
4. Reads README.md
5. Adds contents into Claude context

***

## Example 4 — Block Dangerous Commands

### Config

```json
{
  "hooks": {
    "pre_tool": "./hooks/security-check.sh"
  }
}
```

### Script

```bash
#!/usr/bin/env bash

if [[ "$CLAUDE_TOOL_INPUT" == *"rm -rf"* ]]; then
  echo "Blocked dangerous command"
  exit 1
fi
```

***

## Environment Variables

Hooks can access Claude runtime variables.

Examples:

| Variable             | Description          |
| -------------------- | -------------------- |
| `CLAUDE_USER_PROMPT` | Current user prompt  |
| `CLAUDE_TOOL_NAME`   | Tool being used      |
| `CLAUDE_TOOL_INPUT`  | Tool input           |
| `CLAUDE_PROJECT_DIR` | Current project path |

***

## Best Practices

* Keep hooks fast
* Avoid heavy scripts
* Log failures clearly
* Make hooks idempotent
* Use hooks for guardrails and automation

***

## Mental Model

```
User Prompt
   ↓
Hook Trigger
   ↓
Your Script Runs
   ↓
Claude Continues
```

***

## Good Use Cases

* Auto testing
* Linting
* Security checks
* Injecting docs/context
* CI-style workflows
* Git automation
* Developer productivity tools
