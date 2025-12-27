# Knowledge Update Template

Use this format when proposing knowledge updates through Ada.

## Update Types

### Universal Context Update
**Location**: `shared/context/`
**When**: Information that applies across all agents

Example:
```
Update Type: Universal
File: shared/context/jacob-profile.md
Section: Current Focus Areas
Change: Add "Preparing for Q2 client review cycle"
```

### Agent-Specific Update
**Location**: `agents/[name]/knowledge/`
**When**: Information specific to one agent's domain

Example:
```
Update Type: Agent-Specific
Agent: Basil
File: agents/basil/knowledge/dietary-preferences.md
Section: Temporary Restrictions
Change: Add "Avoiding dairy through January 2026"
```

## Update Flow

1. **User shares information** that should persist
2. **Ada identifies** the update type and location
3. **Ada proposes** the specific change
4. **User confirms** (or modifies)
5. **File is edited** (by user or Claude Code)
6. **Ada confirms** the update is complete

## Proposal Format

When proposing an update, Ada should specify:
- Which file to update
- Which section (if applicable)
- Exact text to add/change/remove
- Why this information is worth persisting

## What to Store

✅ **Store:**
- Dietary preferences and restrictions
- Favorite recipes (with feedback)
- Voice calibrations for content
- Business context changes
- Workflow preferences
- Recurring patterns identified by Sage

❌ **Don't Store:**
- One-off questions
- Temporary context ("I have a meeting in 5 minutes")
- Information explicitly marked as not to remember
- Sensitive information that shouldn't be in files

## Review Cadence

Periodically review knowledge files to:
- Remove outdated temporary restrictions
- Update business context as situations change
- Refine voice guidelines based on accumulated feedback
- Archive patterns that are no longer relevant
