---
description: Quick checkpoint during session
agent: marvin
template: "{file:./.opencode/commands/update.md}"
---

# Update MARVIN Session

## Instructions

Create a mid-session checkpoint without ending the full session.

### Check Current Progress

1. Summarize what's been discussed:
   - Topics covered
   - Decisions made
   - Work completed

2. Update Session Log:
   - File: `sessions/{TODAY}.md` (today's date in YYYY-MM-DD format)
   - Create new session entry with timestamp
   - Format: "### [Time] - Checkpoint: [Summary]"
   - Include: Topics, decisions, in-progress items

### Update State Files

Do NOT modify state files in this command. This is a checkpoint only.

- Skip `state/current.md` updates
- Skip `state/goals.md` updates
- Skip `state/todos.md` updates
- This avoids unintended state changes

### Output Format

Display checkpoint summary:

```markdown
## Checkpoint Saved - [Time]

**Progress Saved**:
- [Item 1]
- [Item 2]
- [Item 3]

**Continuing**:
- Session remains active
- Context preserved
- Next actions remain available
```

## Notes

- This command saves a light checkpoint
- For full session save with state updates, use `/end` command
- Session logs are append-only, so checkpoints don't overwrite
- This command is for quick saves during long sessions

## Safety

- No file deletions in this command
- No state modifications without confirmation
- Read-only operations only for loading state

- Do NOT commit changes in this command
- Use `/commit` command if user wants to commit session
