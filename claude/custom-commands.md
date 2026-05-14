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
* Personal commands (`~/.claude/commands/`) are never committed to git; project commands are.
