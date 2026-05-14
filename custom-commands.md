# Custom commands

## Command Recipes

Drop any of these files into `.claude/commands/` and invoke them by name. Adjust paths and conventions to match your project.

***

### `component.md` — TDD Component Generator

**Invoke:** `/component <description of the component>`

````markdown
---
description: Create a UI component using TDD (test-driven development)
argument-hint: <brief component description>
allowed-tools: Read, Write, Edit, Glob, Bash(npm test:*), Bash(npx vitest:*)
---

## User Input

The user has described the component to build: **$ARGUMENTS**

## Steps

From the description above, determine a PascalCase component name
(e.g. "a card showing user stats" → `UserStatsCard`).

### 1. Write Tests First

Create `tests/components/[ComponentName].test.tsx` with 2–3 simple tests:

- Component renders without crashing
- Key elements are present (roles, text, data-testid)

```tsx
import { render, screen } from "@testing-library/react"
import { describe, it, expect } from "vitest"
import ComponentName from "@/components/ComponentName"

describe("ComponentName", () => {
  it("renders successfully", () => {
    render(<ComponentName />)
    expect(screen.getByRole("...")).toBeInTheDocument()
  })
})
````

#### 2. Run Tests — Expect Failure

```bash
npm test tests/components/[ComponentName].test.tsx
```

#### 3. Create the Component

* `components/[ComponentName]/[ComponentName].tsx`
* `components/[ComponentName]/[ComponentName].module.css`
* `components/[ComponentName]/index.ts` → `export { default } from './[ComponentName]'`

Conventions: no semicolons, CSS Modules, theme colours from `globals.css`.

#### 4. Run Tests — Expect Pass

```bash
npm test tests/components/[ComponentName].test.tsx
```

Iterate until all tests pass.

#### 5. Add to Preview Page

Update `app/(public)/preview/page.tsx` with a labelled section showing the component.

### Rules

* Keep tests minimal — 2–3 assertions is enough.
* Only proceed to the next step when the current step passes.

````

---

## `code-review.md` — PR / Diff Review

**Invoke:** `/code-review`

```markdown
---
description: Comprehensive review of all changes since the last commit
allowed-tools: Read, Grep, Glob, Bash(git diff:*)
---

## Changed Files
!`git diff --name-only HEAD~1`

## Full Diff
!`git diff HEAD~1`

## Review Checklist

Review the above changes for:

1. **Code quality** — readability, naming, unnecessary complexity
2. **Security** — injection, exposed secrets, insecure configs
3. **Performance** — N+1 queries, missing memoisation, large bundles
4. **Test coverage** — untested branches, missing edge cases
5. **Documentation** — missing JSDoc / docstrings, stale comments

Provide specific, actionable feedback organised by priority (critical → suggestion).
````

***

### `security-scan.md` — Security Vulnerability Scan

**Invoke:** `/security-scan`

```markdown
---
description: Scan the codebase for common security vulnerabilities
allowed-tools: Read, Grep, Glob
model: claude-opus-4-6
---

Analyse the codebase for security vulnerabilities including:

- SQL injection risks
- XSS vulnerabilities  
- Exposed credentials or API keys
- Insecure configurations (CORS, CSP, auth)
- Dependency vulnerabilities (check package.json / requirements.txt versions)
- Hardcoded secrets in source files

Report each finding with:
- **Severity:** Critical / High / Medium / Low
- **Location:** file path and line number
- **Description:** what the vulnerability is
- **Fix:** recommended remediation
```

***

### `fix-tests.md` — Run and Fix Failing Tests

**Invoke:** `/fix-tests <optional pattern>`

```markdown
---
description: Run tests and automatically fix any failures
argument-hint: [test-pattern]
allowed-tools: Bash, Read, Edit
---

Run tests matching this pattern: $ARGUMENTS

1. Detect the test framework (Jest, Vitest, pytest, RSpec, etc.).
2. Run the tests with the provided pattern (or all tests if no pattern given).
3. If tests fail, analyse the failure output and fix the root cause.
4. Re-run the tests to verify they pass.
5. Summarise what was broken and what was changed.

Do not change test assertions — fix the implementation instead.
```

***

### `commit.md` — Smart Git Commit

**Invoke:** `/commit <optional message>`

```markdown
---
description: Stage all changes and create a conventional commit
argument-hint: [commit message]
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(git commit:*)
---

1. Run `git status` and `git diff --staged` to understand what is changing.
2. If $ARGUMENTS is provided, use it as the commit message.
   Otherwise, generate a concise Conventional Commits message
   (e.g. `feat: add UserStatsCard component`).
3. Stage all changes: `git add -A`.
4. Commit with the message.
5. Print the commit hash and message.
```

***

### `refactor.md` — Refactor a File

**Invoke:** `/refactor <file path>`

```markdown
---
description: Refactor a file for readability and maintainability
argument-hint: <file-path>
allowed-tools: Read, Edit
---

Refactor the file at: **$ARGUMENTS**

Apply clean code principles:

- Rename unclear variables and functions
- Extract long functions into smaller, single-responsibility ones
- Remove dead code and unnecessary comments
- Improve TypeScript / type annotations where missing
- Ensure consistent formatting

Do **not** change behaviour — only internal structure.
After refactoring, briefly explain each change made.
```

***

### `standup.md` — Daily Standup Summary

**Invoke:** `/standup`

```markdown
---
description: Summarise recent commits for a daily standup
allowed-tools: Bash(git log:*), Bash(git diff:*)
---

!`git log --oneline --since="yesterday" --author="$(git config user.email)"`

Based on the commits above, write a brief standup update:

- **Yesterday:** what was completed
- **Today:** what is planned next (infer from branch name or last commit)
- **Blockers:** note anything that looks incomplete or has a TODO/FIXME

Keep it to 3–5 bullet points. Informal tone.
```

***

### `create-story.md` — Generate a Storybook Story

**Invoke:** `/create-story <ComponentName>`

```markdown
---
description: Generate a Storybook story file for a component
argument-hint: <ComponentName>
allowed-tools: Read, Write, Glob
---

Generate a Storybook story for the component: **$ARGUMENTS**

1. Find the component file under `components/` or `src/components/`.
2. Read it to understand its props.
3. Create `stories/$ARGUMENTS.stories.tsx` with:
   - A `Default` story
   - A story for each significant prop variation
   - CSF3 format (`const meta = { ... } satisfies Meta<typeof Component>`)
4. Use realistic placeholder data — no lorem ipsum for user-facing labels.
```

***

### Tips for Writing Your Own Commands

* **Be specific about output format.** Tell Claude exactly what to produce (file path, section headers, tone).
* **Use numbered steps** for multi-stage workflows — Claude follows them reliably.
* **Embed shell output with `` !`command` ``** to give Claude live context (git diff, file tree, test output).
* **Use `context: fork`** for expensive or destructive commands so they don't pollute your main session.
* **Restrict `allowed-tools`** to the minimum needed — it speeds up permission prompts and makes commands safer.
