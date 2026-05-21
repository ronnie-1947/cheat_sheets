# MCP Servers

**MCP Servers Cheat Sheet for Claude Code**

#### What is MCP?

**Model Context Protocol (MCP)** is an open standard that enables AI assistants like Claude to securely connect to external tools, data sources, and services.

It creates a structured integration layer:

```
Claude Code
    ↓ (MCP)
MCP Server
    ↓
External Tool / Service / Data Source
```

**Common Use Cases:**

* Filesystem access (read, write, search)
* GitHub (repositories, pull requests, issues)
* Databases (PostgreSQL, etc.)
* Collaboration tools (Slack, Notion, Jira)
* Cloud storage (Google Drive)
* Internal company APIs and systems

***

#### Why Use MCP?

**Without MCP**, Claude is limited to the context of the current conversation.\
**With MCP**, Claude gains the ability to interact with real-world tools and data in a controlled, auditable manner. This dramatically expands its usefulness for software development, data analysis, automation, and knowledge work.

**Key Benefits:**

* Direct access to project files and repositories
* Real-time interaction with databases and APIs
* Automated workflows across multiple systems
* Persistent tool discovery and usage within sessions

***

#### MCP Architecture

```
┌─────────────────┐
│   Claude Code   │
└────────┬────────┘
         │ MCP Protocol
         ▼
┌─────────────────┐
│  MCP Server(s)  │
└────────┬────────┘
         │
 ┌───────┼─────────┐
 ▼       ▼         ▼
Filesystem  GitHub  Database
```

Claude communicates with one or more MCP servers via the standardized protocol. Each MCP server acts as a secure bridge to specific external systems.

***

#### Common MCP Servers

| Server         | Primary Purpose                          |
| -------------- | ---------------------------------------- |
| Filesystem     | Read, write, search, and manage files    |
| GitHub         | Repositories, PRs, Issues, and workflows |
| PostgreSQL     | Database queries and management          |
| Slack          | Read and send messages across workspaces |
| Google Drive   | Access and manage documents and files    |
| Notion         | Pages, databases, and wikis              |
| Jira           | Tickets, projects, and agile workflows   |
| Custom Servers | Internal tools and proprietary systems   |

***

#### How Claude Uses MCP

When initialized with MCP servers, Claude follows this process:

1. Connects to all configured MCP servers
2. Discovers available tools and capabilities
3. Selects appropriate tools based on user requests
4. Executes operations and returns structured results

**Example Workflow:**

* **User Prompt**: “Find all TODOs in my project.”
* **Claude Actions**: Uses the Filesystem MCP server to list directories, search files, read relevant content, and summarize findings.

***

#### Installing and Configuring MCP Servers

Most official MCP servers are distributed as npm packages and launched via `npx`.

**Basic Example:**

```bash
npx -y @modelcontextprotocol/server-filesystem ~/projects
```

Claude launches servers using commands you specify in configuration.

**Configuration Methods**

**Option 1: CLI**

```bash
# Add a server
claude mcp add filesystem npx -- -y @modelcontextprotocol/server-filesystem

# List servers
claude mcp list

# Remove a server
claude mcp remove filesystem
```

**Option 2: Project Configuration (`.mcp.json`)**

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "./src"
      ]
    }
  }
}
```

Place the file in your project root.

***

#### Filesystem MCP Server

The most commonly used starting point. It provides controlled access to a specified directory tree.

**Recommended Usage:**

```bash
npx -y @modelcontextprotocol/server-filesystem ~/projects/my-app
```

This grants access **only** to the specified path and its subdirectories — adhering to security best practices.

***

#### Supported MCP Server Types

* **Node.js / npm**: `npx -y <package-name>`
* **Python**: `python server.py`
* **Docker**: `docker run --rm <image>`
* **Standalone Binary**: `./my-mcp-server`

***

#### Example Prompts by Server Type

**Filesystem**

* “Find all TODO comments across the project.”
* “Refactor this codebase to use TypeScript.”
* “Generate API documentation for all routes.”

**GitHub**

* “Summarize all open pull requests.”
* “Review PR #42 and suggest improvements.”
* “Identify bugs reported in the last 7 days.”

**PostgreSQL**

* “Show top 10 customers by revenue this quarter.”
* “Detect and list duplicate records in the users table.”
* “Write an optimized query for monthly sales trends.”

***

#### Debugging MCP

Enable detailed logging:

```bash
claude --mcp-debug
```

Useful for diagnosing:

* Server startup failures
* Tool discovery issues
* Connection or authentication problems
* Permission errors

***

#### Security Best Practices

**1. Principle of Least Privilege**\
Grant the minimum access required.\
**Good**: Limit to `./src` or `~/projects/my-app`\
**Avoid**: Granting access to `/` or user home directories.

**2. Database Permissions**\
Prefer read-only (`SELECT`) access when possible. Avoid granting `DROP`, `DELETE`, or `ALTER` unless explicitly required.

**3. Review Tool Capabilities**\
Before activation, evaluate:

* What data can Claude read?
* What modifications can it make?
* What deletion or destructive actions are possible?

**4. Regular Auditing**\
Periodically review configured servers and their permissions.

***

#### Quick Reference

* **Add Server**: `claude mcp add <name> <command> -- <args>`
* **List Servers**: `claude mcp list`
* **Remove Server**: `claude mcp remove <name>`
* **Debug Mode**: `claude --mcp-debug`

**Example `.mcp.json` Configuration:**

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "./src"]
    }
  }
}
```

***

#### Mental Model

Think of **MCP as a USB-C port for AI**.

Just as USB allows computers to connect to a wide variety of peripherals through a standardized interface, MCP allows Claude to securely connect to diverse tools, data sources, and services through a consistent protocol.

This modular approach makes AI assistants significantly more powerful while maintaining security, control, and transparency.
