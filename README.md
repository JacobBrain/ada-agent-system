# Ada Agent System

Personal AI agent orchestrator with specialized sub-agents for different aspects of life.

## Overview

Ada is the main orchestrator who intelligently routes requests to specialized sub-agents:

| Agent | Name | Specialty |
|-------|------|-----------|
| Admin Assistant | **Iris** | Calendar, tasks, research |
| Sous Chef | **Basil** | Cooking, recipes, meal planning |
| Content Creator | **Muse** | LinkedIn, Substack content |
| Business Coach | **Sage** | Strategic decisions, accountability |

## Quick Start

```bash
cd ada-agent-system
claude
```

Ada will automatically load and be ready to help. Just talk naturally — she'll route to the right agent.

## Usage Examples

**Calendar/Tasks (routes to Iris):**
- "What's on my calendar tomorrow?"
- "Add 'call accountant' to my tasks"
- "Block 2 hours Thursday for deep work"

**Cooking (routes to Basil):**
- [Paste a Pinterest link]
- "What should we make for dinner tonight?"
- "Walk me through this recipe"

**Content (routes to Muse):**
- "Help me write a LinkedIn post about AI implementations"
- "Draft a Substack note about systems vs. willpower"

**Coaching (routes to Sage):**
- "I'm thinking about raising prices"
- "Should I take on this new client?"
- "Help me think through the partnership with Monica"

**Direct Agent Access:**
- "Let me talk to Sage" — switches to direct conversation with Sage
- "Back to Ada" — returns to Ada

## Directory Structure

```
ada-agent-system/
├── CLAUDE.md                 # Ada orchestrator
├── .mcp.json                 # MCP configurations
├── README.md                 # This file
├── shared/
│   └── context/
│       ├── jacob-profile.md  # Universal context
│       └── preferences.md    # Cross-cutting preferences
├── agents/
│   ├── iris/                 # Admin Assistant
│   │   ├── CLAUDE.md
│   │   └── knowledge/
│   ├── basil/                # Sous Chef
│   │   ├── CLAUDE.md
│   │   └── knowledge/
│   ├── muse/                 # Content Creator
│   │   ├── CLAUDE.md
│   │   └── knowledge/
│   └── sage/                 # Business Coach
│       ├── CLAUDE.md
│       └── knowledge/
└── reference/
    └── agent-registry.json   # Agent metadata
```

## Tool Setup

### Google Calendar (for Iris)

The MCP configuration is already in `.mcp.json`. On first use:

1. Run `/mcp` in Claude Code to check status
2. If prompted, authenticate with Google
3. Grant calendar access

### Google Tasks (for Iris)

May require additional setup if MCP isn't available. Check Claude Code for current status.

### Pinterest Parsing (for Basil)

Basil can parse Pinterest recipe links automatically. If parsing fails, he'll ask you to paste the recipe text.

## Knowledge Management

Ada manages knowledge updates across agents. When you share persistent information:

1. Ada identifies which agent's knowledge to update
2. Proposes the change
3. On confirmation, describes the update needed
4. You (or Claude Code) make the edit

**Example:**
```
You: "We're avoiding dairy this month"
Ada: "Should I update Basil's dietary preferences to note the temporary dairy restriction?"
You: "Yes"
Ada: "Done. Basil will avoid dairy suggestions for the rest of the month."
```

## Customization

### Adding Context

- **Universal context**: Edit `shared/context/jacob-profile.md`
- **Preferences**: Edit `shared/context/preferences.md`
- **Agent-specific**: Edit files in `agents/[name]/knowledge/`

### Updating Agent Behavior

Each agent's instructions are in their `CLAUDE.md` file. Edit these to change how they respond.

## Troubleshooting

### Agent not responding correctly

- Check if the request clearly signals the right agent
- Try being more explicit: "Ask Basil about..."
- Switch directly: "Let me talk to Basil"

### MCP tools not working

- Run `/mcp` to check connection status
- May need to re-authenticate with Google
- Check that the MCP server is installed

### Knowledge not persisting

- Ada proposes updates but doesn't make them automatically
- You need to confirm and the file needs to be edited
- Check the relevant knowledge file to verify the update

## Future Enhancements

- [ ] Remote access via SMS (Twilio)
- [ ] Email interface
- [ ] Additional sub-agents as needed
- [ ] More sophisticated tool integrations
