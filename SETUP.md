# Ada Agent System Setup

First-time setup instructions for new machines.

## Prerequisites

- [Claude Code CLI](https://claude.ai/code) installed
- Node.js (for MCP servers via npx)
- Git configured with access to this repo

## Environment Variables

Set these in your system environment before running Ada:

| Variable | Description | How to Get |
|----------|-------------|------------|
| `NOTION_TOKEN` | Notion API integration token | [Create integration](https://www.notion.so/my-integrations) → Copy "Internal Integration Secret" |

### Setting Environment Variables

**Windows (PowerShell - persistent):**
```powershell
[System.Environment]::SetEnvironmentVariable('NOTION_TOKEN', 'your-token-here', 'User')
```

**Windows (Command Prompt - session only):**
```cmd
set NOTION_TOKEN=your-token-here
```

**Mac/Linux:**
```bash
export NOTION_TOKEN=your-token-here
# Or add to ~/.bashrc / ~/.zshrc for persistence
```

## Verification

After setting environment variables, verify everything works:

```bash
# Navigate to project folder
cd path/to/ada-agent-system

# Check MCP connections
claude mcp list
```

All servers should show `✓ Connected`.

## Quick Start (New Machine)

1. Clone the repo
2. Set environment variables (see above)
3. Open terminal in project folder
4. Run `claude` and say "setup" — Ada will verify connections

## Troubleshooting

### MCP Not Connecting

- **Notion**: Verify `NOTION_TOKEN` is set and the integration has access to your workspace pages
- Restart terminal after setting environment variables
- Run `claude mcp list` to see specific error messages

### Missing Environment Variable

Ada will detect missing configuration on startup and guide you through setup.

## Adding New Integrations

When adding a new MCP or API integration:

1. Add server config to `.mcp.json`
2. Document the required environment variable in this file
3. Update Ada's startup check in `CLAUDE.md` if needed
