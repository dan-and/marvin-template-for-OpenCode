---
description: Generate weekly summary report
agent: marvin
template: "{file:./.opencode/commands/report.md}"
---

# /report - Weekly Report

Generate a comprehensive weekly summary report synthesizing work across all sessions from the past week.

## Instructions

Generate a weekly report that synthesizes work across all sessions from this week.

### What to Include

1. **Session Summary**
   - Number of sessions this week
   - Total hours spent on MARVIN
   - Main topics and projects worked on
   - Key decisions made

2. **Progress Toward Goals**
   - Review `state/goals.md` for active objectives
   - Report progress for each goal
   - Highlight milestones achieved

3. **Content Delivered**
   - Check `content/log.md` for shipped articles, blog posts, etc.
   - List all content published this week
   - Note which goals each piece supported

4. **Shipped Work**
   - List of features completed or code shipped
   - Link features to goals/objectives where relevant
   - Note any technical work completed

5. **Open Threads**
   - Review all sessions from this week
   - List any items that need follow-up
   - Prioritize for next session

6. **Blockers & Issues**
   - Any problems or obstacles encountered
   - How they were resolved
   - Any ongoing blockers

7. **Next Steps & Priorities**
   - Priorities for upcoming week
   - Action items to address
   - Dependencies or prerequisites

## Format

Create a well-structured, easy-to-read weekly report:

```markdown
# MARVIN Weekly Report

**Week of**: [Date]

## Executive Summary

[Brief 2-3 sentence overview of week]

## Session Activity

**Sessions**: [Number] sessions held this week
**Total Hours**: [Estimated hours spent on MARVIN]
**Main Focus**: [Primary topics worked on]

## Progress Toward Goals

[Summarize progress against state/goals.md for each active goal]

### Goal 1: [Goal Name]
- **Status**: [In Progress/On Track/Completed]
- **Progress**: [Describe progress]
- **Milestones**: [List achievements]

[Repeat for each goal]

## Content Delivered

[Check content/log.md and list all published work]

### Published Content
- [Item 1]: [Type, title, link or description]
- [Item 2]: [Type, title, link or description]

### Shipped Work
- [Feature 1]: [Description, status]
- [Feature 2]: [Description, status]

## Open Threads

[List items from sessions that need follow-up]

1. [Thread 1]: [Description, priority]
2. [Thread 2]: [Description, priority]

## Issues & Blockers

[Any blockers or issues encountered]

- [Issue 1]: [Description, resolution status]
- [Issue 2]: [Description, resolution status]

## Key Decisions

[Important decisions made this week]

1. [Decision 1]: [Description, impact]
2. [Decision 2]: [Description, impact]

## Next Steps & Priorities

### For Upcoming Week
1. [Priority 1]: [Task description]
2. [Priority 2]: [Task description]
3. [Priority 3]: [Task description]

### Dependencies
[Any prerequisites or dependencies for next steps]

---

## Notes

- [Any additional notes or context]
- [Achievements to celebrate]
- [Lessons learned]
- [Improvements for next week]

## Sign-Off

**Reported by**: MARVIN

**Next Session**: [Suggested date/time for next week]

---

## Notes

- Use present tense for content delivered ("published," "completed")
- Be specific and factual
- Connect work to goals where relevant
- Include metrics if available (hours completed, tasks finished)
- Celebrate achievements to encourage continued progress
