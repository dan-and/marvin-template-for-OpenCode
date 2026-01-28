---
name: marvin
description: Start MARVIN session with context loading and briefing generation
license: MIT
compatibility: opencode
metadata:
  category: session
  user-invocable: true
---

## What I Do

- Load your MARVIN context and generate a briefing
- Manage session lifecycle (start, continue, end)
- Coordinate with other MARVIN capabilities

## When to Use

Use this when user types `/marvin` or starts a new MARVIN session.

## Process

### Step 1: Establish Current Date
Get today's date in ISO format: `YYYY-MM-DD`
Get day of week for context.

### Step 2: Load State Files

Read in this order:
1. `state/current.md` — Active priorities, open threads, recent context
2. `state/goals.md` — Active work goals
3. Check today's session file: `sessions/{TODAY}.md` (if resuming)
4. Read `content/log.md` (if it exists) — Shipped content tracking

### Step 3: Synthesize Context

From loaded state, extract:
- **Top 3 Priorities** — Most important tasks to focus on today
- **Goal Progress** — Check `state/goals.md` tracking
- **Open Threads** — Items in `state/current.md` open threads section
- **Recent Context** — What was discussed in today's previous sessions
- **Content Pace** — If content goals exist, check shipping status

### Step 4: Generate Briefing

Create a concise morning briefing with this structure:

```markdown
Good morning! It's [day], [date].

## Top 3 Priorities
1. [Priority 1 from state/current.md]
2. [Priority 2 from state/current.md]
3. [Priority 3 from state/current.md]

## Goal Progress
[Goal 1]: [Progress from state/goals.md]
[Goal 2]: [Progress from state/goals.md]

## Open Threads
- [Open thread 1 from state/current.md]
- [Open thread 2 from state/current.md]

## Focus for Today
Based on priorities, goals, and open threads, suggest what to work on first.

Let me know when you're ready to work, or if you'd like to adjust priorities.
```

## Step 5: Update Session Log

If today's session file exists:
- Append new session entry with timestamp
- Format: `### [Time] - Session Start`
- Note that this is a new session start, not a resume

If today's session file doesn't exist:
- Create new file: `sessions/{YYYY-MM-DD}.md`
- Start with session start entry
- Format: `### [Time] - Session Start`

## Notes

- If this is a user's first MARVIN session, guide them through initial setup
- If state files are empty, explain that they can start building context
- If no previous sessions exist, mention that this is the start of their journey
- Always be helpful and proactive about next steps

## Integration Connections

Briefing should mention any active integrations configured via MCP servers:
- Google Workspace (if configured)
- Microsoft 365 (if configured)
- Slack or Teams (if configured)
- Note which integrations are active and how they can help with priorities

## Output Format

Keep briefing to 3-4 short paragraphs. Be direct but warm. Use present tense for what's already loaded ("Today is..."), and future tense for suggestions ("Let's focus on...").
