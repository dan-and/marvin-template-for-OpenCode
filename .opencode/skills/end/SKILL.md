---
name: end
description: End MARVIN session, save context, and commit changes
license: MIT
compatibility: opencode
metadata:
  category: session
  user-invocable: true
---

# Session End Skill

End current MARVIN session, save all context to files, and preserve state for future sessions.

## When to Use

When user types `/end` or `/quit` or session is naturally ending.

## Instructions

### 1. Summarize Session
Review the full conversation and extract:
- Topics worked on
- Decisions made
- Content shipped (files modified, code written, etc.)
- Open threads still pending

### 2. Update Content Log
If any content was shipped (articles published, documentation updated, etc.), append to `content/log.md`:
```markdown
## [TODAY]

### [TYPE] "Title" - Description
- URL: link if applicable
- Goal: [related goal from state/goals.md]
```

### 3. Update Session Log

Append to `sessions/{TODAY}.md`:
```markdown
### [TIME] - Session End

**Topics Discussed**
- [topic 1]
- [topic 2]

**Decisions Made**
- [decision 1]

**Content Shipped**
- [content shipped, if any]

**Open Threads**
- [thread 1]

**Next Actions**
- [action 1]

**Session Ended**
- [Time]
```

### 4. Update State Files

Update `state/current.md`:
- Add any new priorities
- Remove completed priorities
- Update open threads with new items
- Update "Last updated" timestamp

## Notes

- Keep summaries concise - this is for session logging, not full reports
- For full reports, use `/report` command
- Session logs are append-only, so each entry adds without overwriting
- Multiple `/end` calls in one day append to same session file

## Safety

- No file deletions in this command
- No state modifications without confirmation (this command only reads context and updates logs)
- Read-only operations only for loading state files
