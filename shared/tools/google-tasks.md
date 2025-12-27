# Google Tasks Integration

Tool for Iris (Admin Assistant) to manage Jacob's task list.

## Current Status

Google Tasks MCP may not be available as a standard package. Implementation options:

### Option 1: Direct API (if MCP unavailable)

Would require:
1. Google Cloud project with Tasks API enabled
2. OAuth credentials
3. Custom integration code

### Option 2: Alternative Task Systems

If Google Tasks integration is complex, could use:
- Simple markdown task file in this repo
- Notion integration (if available)
- Other task MCP if one exists

## Task List Structure

Jacob's current setup:
- **Primary list**: "Jacob Brain's list"
- **Additional lists**: Can be created as needed

## Capabilities Needed

### Read Operations
- List all tasks
- List tasks in specific list
- Get task details
- Filter by due date or status

### Write Operations
- Create new tasks
- Mark tasks complete
- Update task details
- Delete tasks
- Move between lists

## Common Patterns

### Viewing Tasks
```
"What's on my task list?"
"What tasks are due this week?"
"Show me my incomplete tasks"
```

### Adding Tasks
```
"Add 'call accountant' to my tasks"
"Create a task to review Q1 budget, due Friday"
"Remind me to send the proposal"
```

### Completing Tasks
```
"Mark 'call accountant' as done"
"I finished the budget review"
"Complete the proposal task"
```

## Response Format

When showing tasks:
- List tasks with clear formatting
- Show due dates if set
- Indicate completion status
- Offer to add due dates if missing

## Implementation Notes

For Claude Code to help set up:
1. Check if Google Tasks MCP exists
2. If not, evaluate API integration complexity
3. Consider simpler alternatives for v1
4. Can enhance later with proper integration
