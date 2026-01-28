---
name: daily-briefing
description: Generate daily briefing with priorities, progress, and alerts
license: MIT
compatibility: opencode
metadata:
  category: session
  user-invocable: false
---

# Daily Briefing

Generate comprehensive daily briefing with priorities, progress toward goals, and upcoming deadlines. Supports `marvin` skill as part of session start or standalone check-ins.

## Instructions

When invoked by `marvin` skill or when user asks "what's on today", generate briefing with:

1. **Get Date**: Today's date
2. **Load Context**: State files, recent sessions
3. **Review Goals**: Active goals from state/goals.md
4. **Check Content**: Any shipped work from content/log.md
5. **Generate Briefing**: Priorities, alerts, focus

## Process

### 1. Get Current Date
```bash
date +%Y-%m-%d
```
Store for today's session.

### 2. Load State Files

Read in order:
1. `state/current.md` — Active priorities, open threads
2. `state/goals.md` — Goals and progress
3. `sessions/{TODAY}.md` — Recent context
4. Check `content/log.md` (if exists) — Shipped work

### 3. Review Goals

For each goal in state/goals.md:
- Check current month's content log
- Compare days remaining in month
- Report progress

### 4. Generate Briefing

Structure:
```markdown
Good morning! It's {day}, {date}.

## Top 3 Priorities
1. [Priority 1]
2. [Priority 2]
3. [Priority 3]

## Goal Progress
[Goal 1]: [Progress]
[Goal 2]: [Progress]
[Goal 3]: [Progress]

## Open Threads
- [Any open threads]

## Alerts
[Any deadlines approaching]
[Any blockers]

## Focus for Today
[suggested task]

Let me know when you're ready to work.
```

## Output

Present briefing as plain text, not markdown.

## Notes

- Be concise (3-4 paragraphs max)
- Focus on what matters today
- Proactive about deadlines and blockers
- Surface items needing attention
- Connect work to goals

## Safety

No file modifications. Read-only operations only.
