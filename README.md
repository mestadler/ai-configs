# OpenCode Global Configuration

Source of truth for OpenCode configuration — default AGENTS.md, MCP servers, skills, and environment templates.

## Directory Structure

```
opencode/
├── agents/
│   └── default.md          # Default AGENTS.md for new workspaces
├── skills/                   # Shared skills directory
├── mcp/
│   └── servers.json         # MCP server & plugin configurations
├── env/
│   └── default.env.template # Environment variable template
└── logs/
    └── mcp/                 # MCP server logs
```

## Usage

### New Workspace Setup

1. Copy the contents of this directory to your new project workspace
2. Customize `agents/default.md` for project-specific instructions
3. Update `mcp/servers.json` with any workspace-specific MCP configs
4. Copy `env/default.env.template` to `.env` and fill in API keys

### MCP Servers

The `servers.json` file contains configurations for:

- **SearXNG** — Self-hosted search (default, pre-configured)
- **OpenCode Plugins** — Reference for available plugins with setup instructions

### Plugins Reference

| Plugin | Description |
|--------|-------------|
| opencode-pty | Interactive PTY management |
| opencode-websearch-cited | Web search with citations |
| opencode-agent-skills | Dynamic skills discovery |
| opencode-supermemory | Persistent memory |
| superpowers | Software dev workflow |

## Contributing

1. Make changes in this repository
2. Commit with clear messages
3. Push to update the source of truth
4. Copy updated configs to workspaces as needed

## Notes

- Each workspace gets a **hardcopy** of configs (not symlinks)
- Workspaces can diverge from defaults as needed
- This repo remains the source of truth for future projects