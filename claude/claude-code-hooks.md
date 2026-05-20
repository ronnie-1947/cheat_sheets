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

## Advanced Claude Code Hooks

***

## Example Hook Configuration

```json
{
  "hooks": {
    "user_prompt_submit": "./hooks/context-loader.sh",
    "pre_tool_use": "./hooks/security-check.sh",
    "post_tool_use": "./hooks/test-runner.sh",
    "response_complete": "./hooks/notify.sh"
  }
}
```

***

## 1. Context Injection Hooks

### Goal

Automatically inject relevant documentation into Claude context.

### Use Cases

* Load nearest `README.md`
* Inject architecture docs
* Attach API schemas
* Include coding conventions

### Example

```bash
#!/usr/bin/env bash

PROMPT="$CLAUDE_USER_PROMPT"

COMPONENT=$(echo "$PROMPT" | grep -oE 'components/[a-zA-Z0-9_-]+' | head -1)

README="$COMPONENT/README.md"

if [ -f "$README" ]; then
  echo "=== README CONTEXT ==="
  cat "$README"
fi
```

### Flow

```
User Prompt
  ↓
Hook Detects Component
  ↓
README Loaded
  ↓
Claude Gets Better Context
```

***

## 2. Security Guardrail Hooks

### Goal

Prevent dangerous or unauthorized operations.

### Examples

* Block `rm -rf`
* Prevent `.env` modification
* Restrict production access
* Require approval for git push

### Example

```bash
#!/usr/bin/env bash

if [[ "$CLAUDE_TOOL_INPUT" == *"rm -rf"* ]]; then
  echo "Dangerous command blocked"
  exit 1
fi
```

***

## 3. Autonomous Testing Hooks

### Goal

Automatically validate Claude-generated code.

### Workflow

```
Claude Edits Code
  ↓
Hook Runs Tests
  ↓
Failures Returned
  ↓
Claude Fixes Issues
```

### Example

```bash
#!/usr/bin/env bash

npm run lint
npm run test
npm run typecheck
```

***

## 4. Smart Project Routing Hooks

### Goal

Apply different logic based on file or project type.

### Examples

| Project Type | Action           |
| ------------ | ---------------- |
| React        | Run prettier     |
| Python       | Run pytest       |
| Terraform    | Run validate     |
| SQL          | Check migrations |

### Example

```bash
#!/usr/bin/env bash

FILE="$CLAUDE_CHANGED_FILE"

if [[ "$FILE" == *.py ]]; then
  pytest
elif [[ "$FILE" == *.tsx ]]; then
  npm run lint
fi
```

***

## 5. Multi-Agent Hooks

### Goal

Use external systems alongside Claude.

### Examples

* Call another LLM for review
* Trigger CI pipelines
* Run static analysis
* Query internal APIs
* Generate screenshots

### Architecture

```
Claude
  ↓
Hook
  ↓
External Tools / APIs
  ↓
Results Back To Claude
```

***

## 6. Persistent Memory Hooks

### Goal

Simulate long-term memory outside the model.

### Store

* coding preferences
* architecture decisions
* prior bug fixes
* team conventions
* project history

### Example

```bash
echo "$CLAUDE_RESPONSE" >> .claude/history.log
```

***

## 7. Automatic Git + PR Hooks

### Goal

Fully automate development workflow.

### Possible Automations

* create branch
* generate commit message
* open PR
* attach screenshots
* link Jira tickets
* assign reviewers

### Example

```bash
git checkout -b claude/feature-update
git add .
git commit -m "feat: Claude automated update"
```

***

## 8. Semantic File Monitoring Hooks

### Goal

Trigger workflows based on meaning, not just edits.

### Examples

| Change Detected      | Action              |
| -------------------- | ------------------- |
| Auth code changed    | Run security audit  |
| DB schema updated    | Run migration tests |
| API contract changed | Regenerate SDK      |

### Example

```bash
if grep -q "auth" "$CLAUDE_CHANGED_FILE"; then
  npm run security-scan
fi
```

***

## 9. Observability Hooks

### Goal

Track Claude behavior and performance.

### Monitor

* token usage
* latency
* tool usage
* retry loops
* failure patterns
* unsafe attempts

### Example

```bash
echo "$(date) | $CLAUDE_TOOL_NAME" >> metrics.log
```

***

## 10. Self-Improving Agent Hooks

### Goal

Create autonomous improvement loops.

### Flow

```
Claude Generates Code
  ↓
Hooks Evaluate Output
  ↓
Failures Detected
  ↓
Feedback Injected
  ↓
Claude Retries
```

### Example Pipeline

```bash
npm test

if [ $? -ne 0 ]; then
  echo "Tests failed. Retry required."
  exit 1
fi
```

***

## Advanced Hook Architecture

```
User Prompt
    ↓
Context Injection Hook
    ↓
Security Validation Hook
    ↓
Claude Tool Execution
    ↓
Testing Hook
    ↓
Observability Hook
    ↓
Auto Git/PR Hook
    ↓
Final Claude Response
```

***

## Best Practices

### Keep Hooks Fast

Slow hooks reduce Claude responsiveness.

Good:

* cached lookups
* lightweight checks

Avoid:

* huge recursive scans
* expensive builds on every prompt

***

### Make Hooks Deterministic

Hooks should:

* return predictable output
* avoid randomness
* fail safely

***

### Separate Concerns

Recommended structure:

```
hooks/
├── security/
├── testing/
├── context/
├── telemetry/
└── git/
```

***

### Log Everything

Useful for debugging autonomous workflows.

```bash
echo "$(date): hook triggered" >> hooks.log
```

***

## Real Power of Hooks

Hooks transform Claude Code from:

```
AI Assistant
```

into:

```
Programmable Autonomous Engineering System
```
