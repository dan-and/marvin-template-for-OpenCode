---
description: End MARVIN session, save context, and commit changes
agent: marvin
template: "{file:./.opencode/commands/end.md}"
---

# End MARVIN Session

End the current MARVIN session, save all context to files, and preserve state for future sessions.

## Instructions

### Step 1: Summarize Session

Review the full conversation and extract:

1. **Topics discussed** — What was worked on?
   - List all main topics or areas covered
   - Note if work was completed or is in-progress

2. **Decisions made** — What did we decide?
   - List any key decisions or choices made
   - Note reasons for important decisions
   - Document any trade-offs or alternatives considered

3. **Content shipped** — What was delivered?
   - List any files created or modified
   - Note any code written or features implemented
   - Link features to goals or objectives where relevant
   - Note any content published (blog posts, documentation, etc.)

4. **Open threads** — What's still pending?
   - List any items from state/current.md open threads section
   - Identify what follow-ups are needed
   - Note which threads can be closed

5. **Action items** — What's next?
   - List any tasks that need to be done
   - Identify next steps or priorities
   - Note any dependencies or prerequisites

### Step 2: Update Content Log

If any content was shipped or published, append to `content/log.md`:

```markdown
## [Year-Month]

### [Date]
- **[TYPE]** "Title" - [Description]
  - URL: [link if applicable]
  - Goal: [Related goal from state/goals.md]
```

Types:
- **[ARTICLE]** - Blog posts, tutorials, guides
- **[FEATURE]** - New features, functionality, or scripts
- **[CONTENT]** - Newsletter content, social media, videos

### Step 3: Update Session Log

Append a new session entry to today's session file: `sessions/{YYYY-MM-DD}.md`

Session entry format:

```markdown
### [Time] - Session Start

**Topics Discussed**
- [Topic 1]
- [Topic 2]
- [Topic 3]

**Decisions Made**
- [Decision 1]: [Description]
- [Decision 2]: [Description]

**Content Shipped**
- [Item 1]: [Type, title, description]
- [Item 2]: [Type, title, description]

**Open Threads**
- [Thread 1]: [Description, priority]
- [Thread 2]: [Description, priority]

**Next Actions**
- [Action 1]: [Task description]
- [Action 2]: [Task description]

**Session End**
- [Time] - Session end time
- Duration: [Calculated from start to end]
```

### Step 4: Update State Files

#### Update `state/current.md`
1. Add any new priorities to Active Priorities section
2. Remove completed priorities from Active Priorities section
3. Move completed items to Completed section if needed
4. Update Last Updated timestamp

#### Update `state/goals.md`
1. Update progress for any goals worked on
2. Note any milestones achieved
3. Update completion status where relevant

#### Update Open Threads
1. Remove resolved items from open threads section
2. Add any newly identified items that need follow-up
3. Update follow-up dates if applicable

### Step 5: Display Summary

Show user a brief summary of what was saved:

```markdown
## Session Summary

**Session Ended**: [Time]
**Duration**: [Approx duration]

**Saved to**:
- sessions/{YYYY-MM-DD}.md

**State Updated**:
- state/current.md (priorities, open threads)
- state/goals.md (progress)

**Content Logged**:
[If any content shipped]

**Open Threads**:
[Number] threads tracking
[Count] new items added

---

## Notes

- Always save session before closing
- Update state files to maintain continuity across sessions
- Keep sessions chronological
- Track progress toward goals consistently
- Use consistent date formats (YYYY-MM-DD) for session files

- This command should be the last action before closing MARVIN or switching contexts
- Consider offering to create a git commit with `/commit` command
- If no content was shipped and no state changes were made, ask if user wants to commit session log
