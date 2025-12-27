# Google Calendar Integration

Tool for Iris (Admin Assistant) to manage Jacob's calendar.

## Setup

MCP configuration in `.mcp.json`:
```json
{
  "mcpServers": {
    "google-calendar": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-google-calendar"]
    }
  }
}
```

## Authentication

On first use:
1. Run `/mcp` in Claude Code
2. Click the authentication link
3. Sign in with Jacob's personal Google account
4. Grant calendar access permissions

## Capabilities

### Read Operations
- List events for a date range
- Get event details
- Check free/busy status

### Write Operations
- Create new events
- Update existing events
- Delete events

## Calendar Conventions

- **Primary calendar**: Jacob's main personal calendar
- **HOLD events**: Named "HOLD - [meeting name]" for tentative blocks
- **Time zone**: Eastern Time (US)

## Common Patterns

### Viewing Calendar
```
"What's on my calendar tomorrow?"
"What do I have next week?"
"Am I free Thursday afternoon?"
```

### Creating Events
```
"Block 2 hours Thursday for deep work"
"Add a meeting with John at 3pm tomorrow"
"Schedule a dentist appointment for next Tuesday at 2pm"
```

### Modifying Events
```
"Move my 3pm meeting to 4pm"
"Cancel tomorrow's standup"
"Add 30 minutes to the client call"
```

## Response Format

When showing calendar:
- Lead with the date
- List events chronologically
- Include time and duration
- Note significant gaps
- Offer scheduling help when relevant

## Error Handling

If calendar access fails:
- Check MCP connection status
- May need to re-authenticate
- Verify Google account has calendar access enabled
