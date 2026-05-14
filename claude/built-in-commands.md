# Built-in Commands

All commands below are shipped with Claude Code. Type `/help` inside any session to see the full live list (60+ commands available).

***

### Session Management

| Command    | Description                                                |
| ---------- | ---------------------------------------------------------- |
| `/help`    | Show all available commands including custom ones          |
| `/clear`   | Clear the conversation history and start fresh             |
| `/compact` | Compact context to save tokens (summarises older messages) |
| `/context` | Show current context window usage                          |
| `/usage`   | Open the stats dashboard (daily usage, sessions, streaks)  |
| `/cost`    | Alias for `/usage` — shows cost and token stats            |
| `/stats`   | Alias for `/usage` — opens the stats tab                   |
| `/fork`    | Branch the current conversation into a new session         |
| `/branch`  | Alias for `/fork`                                          |

***

### Model & Configuration

| Command                    | Description                              |
| -------------------------- | ---------------------------------------- |
| `/model`                   | Switch the active Claude model           |
| `/model claude-sonnet-4-6` | Switch to Sonnet 4.6                     |
| `/model claude-opus-4-6`   | Switch to Opus 4.6                       |
| `/model claude-haiku-4-5`  | Switch to Haiku 4.5 (fastest, cheapest)  |
| `/config`                  | View or edit Claude Code settings        |
| `/theme`                   | Open theme picker / manage custom themes |

***

### Project Initialisation

| Command | Description                                    |
| ------- | ---------------------------------------------- |
| `/init` | Initialise `CLAUDE.md` for the current project |

> Set `CLAUDE_CODE_NEW_INIT=1` for the interactive setup flow.

***

### Permissions & Security

| Command          | Description                                                                                           |
| ---------------- | ----------------------------------------------------------------------------------------------------- |
| `/allowed-tools` | Analyse recent tool calls and suggest an `allowlist` for `settings.json` to reduce permission prompts |

***

### Team & Onboarding

| Command            | Description                                                                            |
| ------------------ | -------------------------------------------------------------------------------------- |
| `/team-onboarding` | Generate a ramp-up guide from CLAUDE.md, installed skills, hooks, and recent workflows |

> Available from Claude Code v2.1.101+

***

### MCP (Model Context Protocol)

| Command                    | Description                                    |
| -------------------------- | ---------------------------------------------- |
| `/mcp`                     | List connected MCP servers                     |
| `/mcp__<server>__<prompt>` | Invoke an MCP server prompt as a slash command |

**Example:**

```
/mcp__github__list_prs
```

***

### Shortcut Patterns

| Pattern            | What it does                                                                             |
| ------------------ | ---------------------------------------------------------------------------------------- |
| `!<shell command>` | Run a shell command directly, bypassing Claude's conversational mode (uses fewer tokens) |
| `@<file>`          | Reference a file or directory inline in your prompt                                      |

**Examples:**

```bash
!git status           # run shell command directly
!npm test             # run tests without Claude overhead

@src/auth/login.ts    # attach a file to your prompt
@src/components/      # attach a whole directory
```

***

### Notes

* Type `/` to open autocomplete and browse all commands.
* Built-in commands are always available regardless of project.
* Custom commands appear in the same list alongside built-ins.
