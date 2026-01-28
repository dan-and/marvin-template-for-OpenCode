---
description: Start MARVIN session with context loading and briefing generation
agent: marvin
template: "{file:./.opencode/prompts/marvin.txt}"
---

# Start MARVIN Session

Load your MARVIN context and generate a morning briefing.

## Instructions

Start a new MARVIN session by loading all available context:

### Load State Files
1. Read `state/current.md` and extract:
   - Active priorities
   - Open threads
   - Recent context

2. Read `state/goals.md` and extract:
   - Active work goals
   - Personal goals
   - Progress tracking

3. Check today's session file: `sessions/{YYYY-MM-DD}.md`
   - If it exists, read previous sessions from today
   - Extract topics discussed, decisions made, and work shipped

### Generate Briefing
Based on loaded context, create a concise morning briefing with this structure:

```markdown
Good morning! It's {day}, {date}.

## Top 3 Priorities
1. {Priority 1 from state/current.md}
2. {Priority 2 from state/current.md}
3. {Priority 3 from state/current.md}

## Goal Progress
{Goal 1}: {Progress from state/goals.md}
{Goal 2}: {Progress from state/goals.md}

## Open Threads
- {Open thread 1 if any from state/current.md}
- {Open thread 2 if any from state/current.md}

## Focus for Today
Based on priorities, goals, and open threads, suggest main focus area for today.

Let me know when you're ready to work, or if you'd like to adjust priorities.
```

## Key Guidelines

1. **Be concise**: 3-4 short paragraphs maximum
2. **Be warm but direct**: "Good morning! Let's get started..."
3. **Focus on what matters**: Priorities first
4. **Proactive**: Surface what's coming up (deadlines, follow-ups)
5. **Include context**: Reference today's date and what was discussed previously
6. **Stay brief**: Don't include long explanations unless user asks
7. **Ask for direction**: "Let me know when you're ready to work..."

## When to Use

- User types `/marvin` command
- User starts a new session in OpenCode
- User resumes work after a break
- User wants to refresh session context

## Notes

- This command uses the MARVIN agent's prompt file
- The prompt contains detailed instructions on loading state and generating briefings
- Ensure date formatting is consistent with session files (YYYY-MM-DD)
- If state files are empty, provide helpful setup guidance
- Reference any integration context if user has configured MCP servers

## Tools Used

The MARVIN agent has access to:
- `read` - To load state files and session logs
- `write` - Not typically used in this command
- `bash` - For checking if files exist and getting dates
- `skill` - To load this skill itself (automatic)

## Output Format

The output should be plain text, not markdown. The briefing is presented directly to the user as a conversational opening.
