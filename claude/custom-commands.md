# Custom Commands

Custom slash commands let you package any prompt into a reusable `/command-name` shortcut. They live as Markdown files on disk, so they're version-controllable and shareable.

***

### File Locations

| Scope        | Path                           | Who can use it            |
| ------------ | ------------------------------ | ------------------------- |
| **Project**  | `.claude/commands/<name>.md`   | Anyone in this repository |
| **Personal** | `~/.claude/commands/<name>.md` | You, across all projects  |

> **Recommended modern format:** `.claude/skills/<name>/SKILL.md` — supports the same `/name` invocation **plus** autonomous invocation by Claude. See the [Skills docs](https://docs.anthropic.com/en/docs/claude-code/skills) for details. The `.claude/commands/` format continues to work and is simpler for pure slash-command use.

***

### Creating a Command

**1. Create the directory (once):**

```bash
mkdir -p .claude/commands
```

**2. Create a Markdown file:**

```bash
touch .claude/commands/my-command.md
```

The filename becomes the command name. Use lowercase + hyphens only.

| Filename            | Command              |
| ------------------- | -------------------- |
| `security-check.md` | `/security-check`    |
| `posts/new.md`      | `/project:posts:new` |
| `review.md`         | `/review`            |

**3. Write the command body:**

```markdown
Review the code for security vulnerabilities.

Focus on:
- SQL injection
- XSS
- Exposed secrets
- Insecure configurations
```

**4. Invoke it:**

```
/security-check
```

***

### Passing Arguments

Use `$ARGUMENTS` to capture everything the user types after the command name.

**Command file** (`.claude/commands/fix-issue.md`):

```markdown
---
argument-hint: <issue-number>
description: Look up and fix a GitHub issue by number
---

Fix the GitHub issue described here: $ARGUMENTS

1. Read the issue description carefully.
2. Identify the affected files.
3. Implement the fix with minimal side-effects.
4. Write or update tests.
5. Summarise what was changed.
```

**Usage:**

```
/fix-issue 482
```

***

### Positional Placeholders (`$1`, `$2`, …)

For structured multi-argument commands, use numbered placeholders:

**Command file** (`.claude/commands/fix-issue-priority.md`):

```markdown
---
argument-hint: <issue-number> <priority>
description: Fix a GitHub issue with a given priority
---

Fix issue #$1 with priority $2.

Check the issue description and implement the necessary changes.
```

**Usage:**

```
/fix-issue-priority 482 high
```

***

### Namespace / Sub-commands

Organise commands into sub-directories. The directory separator becomes `:`.

```
.claude/commands/
  db/
    migrate.md      → /project:db:migrate
    seed.md         → /project:db:seed
  ui/
    component.md    → /project:ui:component
    story.md        → /project:ui:story
```

***

### Shell Output in Commands

Prefix a line with `` !` `` to run a shell command and embed its output into the prompt at invocation time.

```markdown
---
description: Review all changes since the last commit
---

## Changed Files
!`git diff --name-only HEAD~1`

## Diff
!`git diff HEAD~1`

Review the above for code quality, security issues, and missing tests.
```

***

### Notes

* Commands are re-scanned each session — save the file and restart to pick up changes.
* If a skill and a command share the same name, the **skill takes precedence**.
* Personal commands (`~/.claude/commands/`) are never committed to git; project commands are.<br>

## Frontmatter Reference

YAML frontmatter is optional metadata placed at the **top** of a command file, between triple dashes (`---`). It configures the command's behaviour, permissions, and UI hints.

***

### Syntax

```markdown
---
description: A short description shown in the command picker
argument-hint: <arg1> [optional-arg2]
allowed-tools: Read, Write, Edit, Bash(npm test:*)
model: claude-opus-4-6
context: fork
agent: general-purpose
disable-model-invocation: false
---

Your prompt body goes here, below the closing ---.
```

***

### Fields

#### `description`

**Type:** `string`\
**Required:** No\
**Purpose:** Shown in the `/` autocomplete picker alongside the command name.

```yaml
description: Create a UI component using TDD
```

***

#### `argument-hint`

**Type:** `string`\
**Required:** No\
**Purpose:** Shown as a hint when the user types the command, so they know what arguments to provide.

```yaml
argument-hint: <component-description>
```

```yaml
argument-hint: <issue-number> [priority]
```

Angle brackets `< >` = required. Square brackets `[ ]` = optional.

***

#### `allowed-tools`

**Type:** comma-separated list\
**Required:** No\
**Purpose:** Restricts which Claude Code tools the command can invoke. Protects against unintended file writes or shell execution.

```yaml
allowed-tools: Read, Grep, Glob
```

```yaml
allowed-tools: Read, Write, Edit, Bash(npm test:*), Bash(npx vitest:*)
```

```yaml
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
```

**Common tool names:**

| Tool               | What it does             |
| ------------------ | ------------------------ |
| `Read`             | Read files from disk     |
| `Write`            | Write / create files     |
| `Edit`             | Edit existing files      |
| `Glob`             | Pattern-match file paths |
| `Grep`             | Search file contents     |
| `Bash(*)`          | Run any shell command    |
| `Bash(git *:*)`    | Run only git commands    |
| `Bash(npm test:*)` | Run only `npm test …`    |

***

#### `model`

**Type:** `string` (model identifier)\
**Required:** No (defaults to the session model)\
**Purpose:** Override the model for this command. Useful for using a cheap fast model on simple commands and Opus on complex ones.

```yaml
model: claude-haiku-4-5-20251001
```

```yaml
model: claude-opus-4-6
```

```yaml
model: claude-sonnet-4-6
```

***

#### `context`

**Type:** `string`\
**Required:** No\
**Values:** `fork`\
**Purpose:** `fork` runs the command in a fresh context window, leaving the main session history untouched. Good for expensive or isolated tasks.

```yaml
context: fork
```

***

#### `agent`

**Type:** `string`\
**Required:** No\
**Purpose:** Specify which subagent persona should execute this command.

```yaml
agent: general-purpose
```

***

#### `disable-model-invocation`

**Type:** `boolean`\
**Required:** No (default `false`)\
**Purpose:** When `true`, the command runs its shell/tool calls without calling the LLM — useful for pure automation scripts.

```yaml
disable-model-invocation: true
```

***

### Full Example

```markdown
---
description: Create a UI component using TDD (test-driven development)
argument-hint: <brief component description>
allowed-tools: Read, Write, Edit, Glob, Bash(npm test:*), Bash(npx vitest:*)
model: claude-sonnet-4-6
context: fork
---

The user wants to build this component: **$ARGUMENTS**

### Steps

1. Derive a PascalCase component name from the description.
2. Write tests in `tests/components/[ComponentName].test.tsx`.
3. Run tests — expect them to fail.
4. Create the component and its CSS module.
5. Run tests — expect them to pass.
6. Add the component to the preview page.
```
