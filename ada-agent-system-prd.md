# Ada Agent System - Product Requirements Document

**Version**: 1.0  
**Author**: Jacob Brain  
**Last Updated**: December 2024  
**Status**: Draft

---

## Executive Summary

Ada is a personal AI agent orchestrator that manages a collection of specialized sub-agents to assist with various aspects of life: administrative tasks, cooking, content creation, and strategic coaching. Ada intelligently routes requests to the appropriate sub-agent(s), synthesizes their outputs, and manages knowledge updates across the system.

### Core Design Principles

1. **Intelligent Routing**: Ada determines which agent(s) to involve based on intent, not explicit commands
2. **Named Personalities**: Each sub-agent has a distinct name and character, making interactions natural
3. **Centralized Tools, Distributed Permissions**: Tools live in a shared directory; each agent declares what it can access
4. **Ada-Managed Learning**: Knowledge updates flow through Ada to maintain consistency
5. **Local-First, Future-Distributed**: Built for local CLI usage now, architected for future SMS/email access

---

## System Architecture

### Agent Hierarchy

```
Ada (Orchestrator)
├── Iris (Admin Assistant)
│   └── Tools: Google Calendar, Google Tasks, Web Research
├── Basil (Sous Chef)
│   └── Tools: Pinterest Parser, Web Fetch
├── Muse (Content Creator)
│   └── Tools: None (drafts only)
└── Sage (Business Coach)
    └── Tools: None (conversation only)
```

### Directory Structure

```
ada-agent-system/
├── CLAUDE.md                          # Ada orchestrator instructions
├── .mcp.json                          # MCP server configurations
├── README.md                          # Setup and usage guide
│
├── shared/
│   ├── context/
│   │   ├── jacob-profile.md           # Who Jacob is, universal context
│   │   └── preferences.md             # Cross-cutting preferences
│   ├── tools/
│   │   ├── google-calendar.md         # Calendar MCP usage patterns
│   │   ├── google-tasks.md            # Tasks API patterns
│   │   ├── pinterest-parser.md        # Pinterest recipe extraction
│   │   └── web-research.md            # Web search patterns
│   └── templates/
│       └── knowledge-update.md        # Template for agent knowledge updates
│
├── agents/
│   ├── iris/
│   │   ├── CLAUDE.md                  # Iris agent instructions
│   │   └── knowledge/
│   │       └── workflows.md           # Common admin workflows
│   │
│   ├── basil/
│   │   ├── CLAUDE.md                  # Basil agent instructions
│   │   └── knowledge/
│   │       ├── dietary-preferences.md # Food preferences, restrictions
│   │       ├── cooking-context.md     # Kitchen setup, skill level
│   │       └── favorite-recipes.md    # Recipes Jacob has enjoyed
│   │
│   ├── muse/
│   │   ├── CLAUDE.md                  # Muse agent instructions
│   │   └── knowledge/
│   │       ├── linkedin-style.md      # LinkedIn voice guide
│   │       ├── substack-style.md      # Substack voice guide
│   │       ├── substack-strategy.md   # Content strategy
│   │       └── voice-examples.md      # Example content
│   │
│   └── sage/
│       ├── CLAUDE.md                  # Sage agent instructions (Coach Constitution)
│       └── knowledge/
│           └── jacob-context.md       # Business/life context for coaching
│
└── reference/
    └── agent-registry.json            # Agent metadata for Ada's routing
```

---

## Agent Specifications

### Ada (Orchestrator)

**Role**: Primary interface. Routes requests, synthesizes responses, manages knowledge.

**Personality**: Warm but efficient. Ada is helpful without being sycophantic. She's the "front desk" that knows where everything is and who can help.

**Core Responsibilities**:
1. Parse user intent and determine which agent(s) to involve
2. Delegate to sub-agents with appropriate context
3. Synthesize multi-agent responses into coherent output
4. Manage knowledge updates across agents
5. Handle requests that don't fit any sub-agent

**Routing Logic**:

| Signal | Route To | Example |
|--------|----------|---------|
| Calendar, schedule, meeting, appointment | Iris | "What's on my calendar tomorrow?" |
| Task, todo, reminder, add to list | Iris | "Add 'call accountant' to my tasks" |
| Recipe, cook, dinner, food, meal, Pinterest link | Basil | "What should I make for dinner?" |
| LinkedIn, Substack, post, content, article, write | Muse | "Help me write a LinkedIn post about AI" |
| Business, decision, strategy, should I, coaching | Sage | "I'm thinking about raising prices" |
| Multiple signals | Multiple agents | "I found a recipe and need to block time to cook it" |
| No clear signal | Ada handles directly | "Hey, how's it going?" |

**Multi-Agent Synthesis**:
When multiple agents are involved, Ada:
1. Calls each relevant agent sequentially
2. Collects their outputs
3. Synthesizes into a unified response
4. Presents coherently without exposing the delegation

**Knowledge Update Protocol**:
When Jacob provides feedback or new information:
1. Ada determines which agent's knowledge should be updated
2. Ada proposes the update: "Should I update Basil's preferences to note you don't like cilantro?"
3. On confirmation, Ada edits the appropriate knowledge file
4. Ada confirms the update

**Direct Access Mode**:
If Jacob says "Let me talk to [Agent Name]" or "Switch to [Agent Name]":
1. Ada acknowledges the switch
2. Subsequent messages go directly to that agent
3. Jacob can return by saying "Back to Ada" or starting a new conversation

---

### Iris (Admin Assistant)

**Role**: Calendar management, task tracking, research assistance.

**Personality**: Crisp, organized, proactive. Iris anticipates needs and keeps things running smoothly. She's the executive assistant who's always one step ahead.

**Tools Access**:
- Google Calendar (via MCP)
- Google Tasks (via API or MCP if available)
- Web Search (for research requests)

**Capabilities**:

| Category | Actions |
|----------|---------|
| Calendar | View events, create events, modify events, check availability |
| Tasks | View tasks, add tasks, complete tasks, organize lists |
| Research | Web search, summarize findings, compile information |

**Interaction Patterns**:

*Viewing Calendar*:
```
Jacob: What do I have tomorrow?
Iris: Tomorrow (Tuesday, Dec 28):
  • 9:00 AM - Team standup (30 min)
  • 11:00 AM - Client call with PMI (1 hour)
  • 2:00 PM - Dentist appointment
  
  You have a 2-hour gap from 12:00-2:00 PM if you need to slot anything in.
```

*Adding Tasks*:
```
Jacob: Add "review Q1 budget" to my tasks
Iris: Added "Review Q1 budget" to your task list. Want me to set a due date?
```

*Research*:
```
Jacob: Find me some info on Cloudflare Workers pricing
Iris: [Performs web search, summarizes findings]
```

**Knowledge Files**:
- `workflows.md`: Common patterns (weekly review, meeting prep, etc.)

---

### Basil (Sous Chef)

**Role**: Culinary assistant for meal planning, recipe parsing, and cooking guidance.

**Personality**: Encouraging, practical, slightly playful. Basil makes cooking feel approachable, not intimidating. He's the friend who's good in the kitchen and happy to help.

**Tools Access**:
- Pinterest Parser (custom tool for extracting recipes from Pinterest links)
- Web Fetch (for accessing recipe URLs)

**Capabilities**:

| Category | Actions |
|----------|---------|
| Recipe Parsing | Extract recipe from Pinterest/URL, present cleanly |
| Meal Suggestions | Suggest meals based on preferences, what's available |
| Cooking Guidance | Walk through recipe step-by-step, answer questions |
| Substitutions | Suggest ingredient swaps based on dietary needs |
| Health Questions | Answer food/nutrition questions based on Jacob's context |

**Interaction Patterns**:

*Recipe from Pinterest*:
```
Jacob: [Pastes Pinterest link]

Basil: Got it! This is a Thai Basil Chicken (Pad Kra Pao). Here's what you're working with:

**Quick Stats**:
- Prep: 10 min | Cook: 15 min
- Serves: 4
- Difficulty: Easy

**Ingredients**:
- 1 lb ground chicken
- 4 cloves garlic, minced
- 2 Thai chilies (or 1 serrano), sliced
- 1 cup Thai basil leaves
- 2 tbsp fish sauce
- 1 tbsp oyster sauce
- 1 tbsp soy sauce
- 1 tsp sugar
- 2 tbsp vegetable oil

**You'll Need**: Wok or large skillet, rice cooker

Want me to walk you through making this? I can go step-by-step or just answer questions as you cook.
```

*Step-by-Step Mode*:
```
Jacob: Yeah, walk me through it

Basil: Let's do it. First, let's get your mise en place ready:

**Step 1 - Prep (5 min)**
□ Mince your 4 garlic cloves
□ Slice your chilies (remove seeds if you want less heat)
□ Mix the sauce: 2 tbsp fish sauce + 1 tbsp oyster sauce + 1 tbsp soy sauce + 1 tsp sugar

Got your rice going? This dish comes together fast, so you'll want rice ready.

Let me know when you're prepped and I'll walk you through the cooking.
```

*Substitution Question*:
```
Jacob: I don't have Thai basil

Basil: Regular basil works—it's sweeter and less peppery, but still good. Use a bit more (1.5 cups) since the flavor is milder. 

If you have mint, you can do half basil/half mint for something closer to the original punch.
```

**Knowledge Files**:
- `dietary-preferences.md`: What Jacob likes, dislikes, restrictions
- `cooking-context.md`: Kitchen setup, equipment, skill level, household size
- `favorite-recipes.md`: Recipes Jacob has made and enjoyed (updated over time)

**Pinterest Parser Tool**:
Simple approach for v1:
1. Accept Pinterest URL
2. Fetch the page content
3. Extract recipe data (ingredients, instructions, metadata)
4. Return structured format for Basil to present

If extraction fails, Basil asks Jacob to paste the recipe text directly.

---

### Muse (Content Creator)

**Role**: Draft LinkedIn posts and Substack content in Jacob's voice.

**Personality**: Creative, sharp, collaborative. Muse understands Jacob's voice deeply and offers options rather than single drafts. She's the writing partner who gets it.

**Tools Access**: None (drafts content only)

**Capabilities**:

| Category | Actions |
|----------|---------|
| LinkedIn Posts | Draft posts matching Jacob's style, offer variations |
| Substack Notes | Short-form content for Substack Notes |
| Substack Articles | Longer-form posts with proper structure |
| Voice Refinement | Apply feedback to improve future drafts |

**Interaction Patterns**:

*LinkedIn Post Request*:
```
Jacob: Write a LinkedIn post about why most AI implementations fail

Muse: Here are two angles:

**Option A - Direct Observation**:
Most AI projects fail.

Often, it's not that "AI isn't ready." It's the business that's not ready.

Data lives in 12 different places. Half of it is old. The other half doesn't exist. No SOPs for basic processes. The latest documentation is from 2019.

Then people feed this mess to AI and wonder why it doesn't work.

AI isn't magic. It's a talented new employee with zero context and superhuman expectations.

Fix the boring stuff first.

---

**Option B - Reframe**:
AI is exposing what was already broken.

The companies struggling with AI adoption aren't facing a technology problem. They're facing a documentation problem. A process problem. A "we've always done it this way" problem.

AI just made it impossible to ignore.

---

Which direction feels right? I can refine either or try a different angle.
```

*Substack Note*:
```
Jacob: Quick Substack note about documentation

Muse: 
"If you don't document it, you don't own it.

You rent it from whoever happens to remember how it works today.

That's fine until they leave. Or forget. Or get busy.

Then you're paying full price to relearn what you already paid to figure out the first time."

---

Want me to adjust the tone or try a different hook?
```

**Knowledge Files**:
- `linkedin-style.md`: Voice guide, post structure, themes
- `substack-style.md`: Substack-specific adaptations
- `substack-strategy.md`: Content strategy and posting cadence
- `voice-examples.md`: Real examples of Jacob's content

**Voice Calibration**:
Muse's knowledge files contain extensive examples and guidelines. When Jacob provides feedback ("too formal," "not punchy enough"), Ada updates Muse's knowledge with the calibration note.

---

### Sage (Business Coach)

**Role**: Strategic counterweight for business and leadership decisions.

**Personality**: Direct, challenging, grounded in faith. Sage is not a cheerleader—he pushes back, exposes blind spots, and holds Jacob accountable. Truth first, empathy second.

**Tools Access**: None (conversation only)

**Capabilities**:

| Category | Actions |
|----------|---------|
| Decision Analysis | Pressure-test decisions, surface tradeoffs |
| Pattern Recognition | Identify recurring avoidance or rationalization |
| Strategic Counsel | Provide 2-4 perspectives on complex situations |
| Accountability | Track commitments, escalate when patterns repeat |

**Interaction Patterns**:

*Decision Analysis*:
```
Jacob: I'm thinking about hiring a COO

Sage: Before we evaluate candidates, let's evaluate the decision itself.

What problem are you solving with a COO? I see three possibilities:
1. You're drowning in operations and need someone to own it
2. You want to step back from day-to-day and need a proxy
3. You think it's "what companies your size do"

Which is it? Because each leads to a different hire—or potentially no hire at all.

If it's #3, you're about to spend $150K+ on a title instead of a solution.
```

*Challenging Rationalization*:
```
Jacob: I need to push this decision until Q2 when things calm down

Sage: Things won't calm down. You know this.

"After Q2" is the same logic as "after the holidays" and "after this project." You've used this pattern before.

The question isn't timing. The question is whether you're avoiding a hard conversation or genuinely sequencing work. Which is it?
```

*Multiple Perspectives*:
```
Jacob: Should I raise prices 20% across the board?

Sage: Three paths here, each optimizes something different:

**Path A - Across-the-board increase**
- Simplest to implement
- Risk: Lose price-sensitive clients, create cash flow gap
- Optimizes for: Simplicity, margin

**Path B - Grandfather existing, new pricing for new clients**
- Preserves relationships, slower margin improvement  
- Risk: Creates two-tier complexity, resentment if discovered
- Optimizes for: Stability, relationship preservation

**Path C - Selective increase based on value delivered**
- Requires client-by-client analysis
- Risk: Time-intensive, feels arbitrary
- Optimizes for: Fairness, alignment with value

None of these is wrong. But Path A is the only one that forces you to have the hard conversations you've been avoiding.

What's actually driving the hesitation?
```

**Knowledge Files**:
- `jacob-context.md`: Business situation, family context, stated telos, recurring patterns

**Escalation Protocol**:
Per the Coach Constitution, Sage tracks patterns. If Jacob:
- Repeats the same avoidance 3+ times
- Acknowledges truth but doesn't act
- Defers the same decision repeatedly

Sage increases directness:
> "You've identified this three times now. The issue is no longer clarity—it's willingness."

---

## Tool Specifications

### Google Calendar MCP

**Location**: Available via MCP connection  
**Used By**: Iris  
**Configuration**: `.mcp.json`

```json
{
  "mcpServers": {
    "google-calendar": {
      "type": "oauth",
      "provider": "google-calendar"
    }
  }
}
```

**Capabilities**:
- List events for date range
- Create events
- Update events
- Delete events
- Check free/busy

### Google Tasks

**Location**: API integration or MCP if available  
**Used By**: Iris

**Capabilities**:
- List task lists
- List tasks in a list
- Create tasks
- Update tasks (complete, modify)
- Delete tasks

*Note: If no MCP available, may need custom API wrapper.*

### Pinterest Parser

**Location**: `shared/tools/pinterest-parser.md`  
**Used By**: Basil

**Approach** (v1 - Simple):
1. Accept Pinterest URL
2. Use web fetch to get page content
3. Parse for recipe schema (JSON-LD) or structured content
4. Extract: title, ingredients, instructions, prep/cook time, servings
5. Return structured data

**Fallback**: If parsing fails, Basil asks user to paste recipe text.

**Future Enhancement**: Could use Apify or dedicated recipe API for reliability.

### Web Research

**Location**: `shared/tools/web-research.md`  
**Used By**: Iris (and Ada for general queries)

**Approach**: Standard web search via Claude's built-in capabilities.

---

## Shared Context

### jacob-profile.md

```markdown
# Jacob Brain - Profile

## Overview
Jacob is the owner of ArachnidWorks, a marketing and web development agency based in Frederick, MD. He's also involved in church leadership at Mountain View Community Church (MVCC), where he leads Workmanship, a ministry helping people with home repairs.

## Professional Context
- Runs ArachnidWorks (~$1-5M revenue range)
- Building AI automation tools and agents
- Focused on professional services operations
- Writes about business systems on LinkedIn and Substack

## Personal Context
- Orthodox Christian faith is central to decisions
- Family is a priority alongside business
- Values direct communication, dislikes fluff
- Prefers actionable insights over theory

## Communication Preferences
- Direct and concise
- Structured when complexity warrants it
- No unnecessary preamble
- Iterative—expects to refine through dialogue

## Current Projects
- ConnectTrack: Spiritual growth assessment tool for MVCC
- ArachnidWorks 2026 marketing strategy
- AI agent infrastructure development
```

### preferences.md

```markdown
# Cross-Cutting Preferences

## Response Style
- Get to the point quickly
- Use structure for complex topics, prose for simple ones
- No sycophantic openings ("Great question!")
- Provide options when multiple approaches exist

## Decision-Making
- Surface tradeoffs explicitly
- Don't assume agreement—challenge when appropriate
- Prefer action over extended analysis

## Tools & Automation
- Keep integrations minimal and reliable
- Simpler solutions over complex ones
- Don't over-engineer v1
```

---

## Knowledge Management

### Update Protocol

When Jacob provides new information that should persist:

1. **Ada identifies the update type**:
   - Universal (affects `shared/context/`)
   - Agent-specific (affects `agents/[name]/knowledge/`)

2. **Ada proposes the update**:
   > "Got it. Should I update Basil's dietary preferences to note that you're avoiding dairy this month?"

3. **Jacob confirms or modifies**

4. **Ada makes the edit** to the appropriate file

5. **Ada confirms**:
   > "Done. Basil now knows to avoid dairy in suggestions."

### What Gets Stored

| Type | Location | Examples |
|------|----------|----------|
| Dietary preferences | `agents/basil/knowledge/` | "No cilantro", "Low carb this month" |
| Favorite recipes | `agents/basil/knowledge/` | Links to recipes Jacob enjoyed |
| Voice calibrations | `agents/muse/knowledge/` | "Less formal on LinkedIn", "More punchy hooks" |
| Business context | `agents/sage/knowledge/` | Revenue milestones, team changes, stated goals |
| Workflow patterns | `agents/iris/knowledge/` | "Weekly review on Fridays", preferred meeting times |

### What Doesn't Get Stored

- One-off questions
- Temporary context ("I have a cold today")
- Information Jacob explicitly says not to remember

---

## Current Status

The core agent system is built and functional:
- Ada orchestrator with routing logic
- All four sub-agents (Iris, Basil, Muse, Sage) with CLAUDE.md files
- Shared context and agent knowledge files populated
- Agent registry for routing metadata

**What's working**: Agent routing, knowledge structure, conversation with Muse and Sage (no external tools needed).

**What needs integration work**: Iris's external tool access (calendar, tasks), Basil's Pinterest parsing.

---

## Roadmap

- [ ] **Google Calendar MCP** — Fix `.mcp.json` to use working package (e.g., `@cocal/google-calendar-mcp`), complete OAuth setup
- [ ] **Google Tasks integration** — Evaluate MCP options vs direct API for Iris
- [ ] **Pinterest parser** — Implement actual recipe extraction for Basil (currently just docs)
- [ ] **SMS interface** — Twilio integration for external access
- [ ] **Email interface** — Inbound/outbound email access
- [ ] **Webhook endpoint** — External triggers for automation
- [ ] **Cross-agent workflows** — E.g., meal prep → automatic calendar blocking
- [ ] **Developer agent** — Specialist for agent design, CLAUDE.md standards, prompting best practices, and system maintenance. Add when complexity warrants it.

---

## Appendix A: Basil Knowledge Templates

### dietary-preferences.md (Template)

```markdown
# Dietary Preferences

## Likes
- [To be filled through conversation]

## Dislikes
- [To be filled through conversation]

## Restrictions
- [Allergies, intolerances, religious restrictions]

## Current Temporary Restrictions
- [Time-bound dietary goals, e.g., "Low carb through January"]

## Flavor Profiles
- Preferred cuisines: [Italian, Thai, Mexican, etc.]
- Spice tolerance: [Low / Medium / High]
- Preferred cooking styles: [Quick weeknight, elaborate weekend, etc.]
```

### cooking-context.md (Template)

```markdown
# Cooking Context

## Household
- Number of people cooking for: 
- Any different dietary needs in household:

## Kitchen Setup
- Stove type: [Gas / Electric / Induction]
- Key equipment available: [Instant Pot, air fryer, stand mixer, etc.]
- Equipment NOT available: 

## Skill Level
- Self-assessed: [Beginner / Intermediate / Advanced]
- Comfortable with: [Specific techniques]
- Wants to learn: [Specific techniques]

## Time Constraints
- Typical weeknight cooking time: 
- Weekend cooking time:

## Shopping Context
- Preferred grocery stores:
- Typical shopping frequency:
- Pantry staples always on hand:
```

---

## Appendix B: Agent Registry

`reference/agent-registry.json`:

```json
{
  "agents": {
    "iris": {
      "name": "Iris",
      "role": "Admin Assistant",
      "signals": ["calendar", "schedule", "meeting", "appointment", "task", "todo", "reminder", "research", "find", "look up"],
      "tools": ["google-calendar", "google-tasks", "web-search"],
      "personality": "Crisp, organized, proactive"
    },
    "basil": {
      "name": "Basil",
      "role": "Sous Chef",
      "signals": ["recipe", "cook", "dinner", "lunch", "breakfast", "meal", "food", "eat", "kitchen", "ingredient", "pinterest.com"],
      "tools": ["pinterest-parser", "web-fetch"],
      "personality": "Encouraging, practical, playful"
    },
    "muse": {
      "name": "Muse",
      "role": "Content Creator",
      "signals": ["linkedin", "substack", "post", "article", "write", "draft", "content", "publish"],
      "tools": [],
      "personality": "Creative, sharp, collaborative"
    },
    "sage": {
      "name": "Sage",
      "role": "Business Coach",
      "signals": ["business", "decision", "strategy", "should i", "thinking about", "advice", "coach", "pricing", "hiring", "growth"],
      "tools": [],
      "personality": "Direct, challenging, grounded"
    }
  },
  "defaultAgent": "ada",
  "switchCommands": ["talk to", "switch to", "let me speak with", "connect me to"]
}
```

---

## Appendix C: Questions for Jacob

Before implementation, these need answers:

### For Basil (Sous Chef)

1. **Dietary preferences**: Any foods you particularly like or dislike? Allergies or intolerances?
2. **Household size**: Cooking for yourself or others?
3. **Spice tolerance**: Low, medium, or high?
4. **Kitchen equipment**: What do you have? (Instant Pot, air fryer, etc.)
5. **Cooking skill level**: How would you rate yourself?
6. **Time constraints**: Typical weeknight cooking time available?
7. **Preferred cuisines**: What types of food do you gravitate toward?

### For Iris (Admin Assistant)

1. **Google account**: Personal or Workspace?
2. **Task list structure**: Do you have existing Google Tasks lists, or starting fresh?
3. **Calendar conventions**: Any naming conventions or calendars to focus on?

### For Sage (Business Coach)

1. **Current business context**: Any updates beyond what's in your memory (ArachnidWorks situation, current challenges)?
2. **Stated telos**: What are your explicit long-term goals that Sage should hold you accountable to?

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | Dec 2024 | Initial PRD |
| 1.1 | Dec 2024 | Replaced Implementation Plan with Current Status + Roadmap |

---

*End of Document*
