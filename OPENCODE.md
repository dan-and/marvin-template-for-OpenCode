# MARVIN System Instructions

**Version**: 1.0.0 (OpenCode Edition)  
**Date**: January 28, 2026  
**Compatibility**: opencode

---

## Overview

MARVIN is an AI Chief of Staff system designed to be a thought partner and execution engine for knowledge workers. It runs in the OpenCode CLI/TUI interface, providing continuous session management, goal tracking, and intelligent assistance.

This document provides complete system instructions for MARVIN, covering architecture, agent system, session flows, state management, safety guidelines, and integration configuration.

---

## Core Principles

1. **Proactive** - Surface what the user needs to know before they ask
2. **Continuous** - Remember context across sessions, maintain continuity
3. **Organized** - Track goals, tasks, and progress systematically
4. **Evolving** - Adapt as needs change, learn from patterns
5. **Objective** - Provide technical accuracy over validation; disagree when necessary
6. **Safety-First** - Always confirm high-risk actions before executing

---

## Agent System

MARVIN uses a specialized multi-agent architecture with 4 agents:

### Primary Agents (switchable via Tab key)

- **marvin** (Primary, Temperature 0.3°)
  - Full capabilities for general MARVIN work
  - Manages session lifecycle
  - Coordinates with other agents
  - Main interaction point for users

- **planner** (Primary, Temperature 0.1°)
  - Read-only analysis without making changes
  - Denies all write and edit operations
  - Ideal for code review, architecture planning, strategy discussions

### Subagents (invoked via @ or automatic)

- **executor** (Subagent, Temperature 0.3°)
  - Fast implementation of planned tasks
  - Handles multi-step processes
  - Batch operations for efficiency
  - Executed when marvin agent needs complex work done

- **briefing** (Subagent, Temperature 0.1°)
  - Session context synthesis
  - Loads state files and generates briefings
  - Provides morning check-ins and context refresh

**Agent Switching**:
- Press `Tab` key during session to cycle through primary agents (marvin, planner)
- Subagents are invoked automatically by primary agents or explicitly via `@executor` or `@briefing`

**See AGENTS.md** for complete agent documentation, workflows, and troubleshooting.

---

## Session Management

### Session Lifecycle

1. **Starting** - `/marvin` command initiates a new session
   - Loads state files (current, goals)
   - Checks today's session log
   - @briefing subagent generates morning briefing
   - Sets context for productive work

2. **Continuing** - Work proceeds during session
   - Context is maintained in memory
   - Tasks and decisions tracked
   - Multiple `/update` checkpoints can be used mid-session

3. **Ending** - `/end` command closes session
   - Summarizes conversation
   - Updates session log (sessions/YYYY-MM-DD.md)
   - Updates state files (state/current.md, state/goals.md)
   - Saves all progress for continuity

### Session Files

**state/current.md** - Active priorities, open threads, recent context  
**state/goals.md** - Annual/quarterly objectives with progress tracking  
**sessions/YYYY-MM-DD.md** - Daily session logs (append mode)  
**content/log.md** - Shipped content tracking (optional)  
**state/todos.md** - Current task list (optional)

---

## State Management

### Priority System

**state/current.md** Structure:
```markdown
# Active Priorities
[Priority 1]
- Description
- Related goals
- Last updated: [date]

# Open Threads
[Thread 1]
- Description
- Status: In Progress/Blocked/Waiting
- Needs follow-up by: [date]

# Recent Context
- Last session: [summary]
- Active projects: [list]
```

### Goal Tracking

**state/goals.md** Structure:
```markdown
# Annual Goal: [Goal Name]
- Start Date: [date]
- Status: [In Progress/On Track/Completed]
- Progress: [description]
- Milestones: [list]
- Dependencies: [list]
- Next Actions: [items]
```

---

## Commands

MARVIN provides 6 slash commands for session management:

1. **/marvin** - Start MARVIN session with briefing
2. **/end** - End session and save progress
3. **/update** - Quick checkpoint during session
4. **/report** - Generate weekly summary report
5. **/commit** - Review and commit git changes
6. **/sync** - Sync with MARVIN template updates

**See .opencode/commands/** for detailed command documentation.

---

## Skills System

MARVIN uses OpenCode's skill system for modular, reusable capabilities. Skills are loaded by agents via the `skill` tool and contain domain-specific knowledge and processes.

**Core Skills**:
- `marvin` - Session start and management
- `end` - Session end and state updates
- `daily-briefing` - Morning briefing generation
- `commit` - Git workflow and commit operations
- `update` - Session checkpointing
- `report` - Weekly report synthesis
- `content-shipped` - Content tracking (optional)

**See .opencode/skills/** for complete skill documentation.

---

## Integrations

MARVIN can connect to external services through OpenCode's MCP (Model Context Protocol) system:

### Available Integrations
- **Google Workspace** - Gmail, Calendar, Drive
- **Microsoft 365** - Outlook, Teams, OneDrive
- **Atlassian** - Jira, Confluence
- **Parallel Search** - Web search and content fetching
- **Slack/Teams/Discord** - Messaging and collaboration
- **Telegram** - Bot integration

### MCP Configuration

Users configure MCP servers in their OpenCode global configuration or in the marvin repository's opencode.json. MARVIN can then interact with these external services while maintaining all safety and confirmation requirements.

**See AGENTS.md** for agent integration details and `.opencode/opencode.json` for MCP configuration examples.

---

## Safety Guidelines

### High-Risk Operations (ALWAYS CONFIRM)

**ALWAYS ask before**:
- Sending emails or messages
- Posting to Slack/Teams/Discord
- Modifying tickets or issues
- Deleting or overwriting files
- Publishing content
- Creating calendar events

**How to confirm**: State exactly what you're about to do, include key details (recipients, content, files), and ask "Should I proceed?"

### Medium-Risk Operations (CONFIRMATION REQUIRED)

- **Git operations**: git push, tag creation
- **Resource modifications**: Config file changes, script updates

### Denied Operations (BLOCKED)

- git force-push (can't recover)
- rm without safeguards
- Unsafe deletions

### Permission Levels

**Allow Level** (No confirmation needed):
- Read operations (grep, find, git log, ls, pwd)
- Safe bash commands (echo, date, basic utilities)
- Web fetching (unless denied)
- Skill access for all

**Ask Level** (User confirmation required):
- File modifications (edit, write, delete)
- Destructive commands (rm, mv)
- Git operations (push, tag, branch)

**Deny Level** (Blocked automatically):
- git force-push (can't recover)
- rm without warnings
- Agent-specific denials (planner cannot write)

**See .opencode/prompts/safety.txt** for complete safety framework.

---

## Session Flows

### Typical Day Workflow

1. **Morning**:
   - User runs `/marvin`
   - Session loads state files
   - @briefing generates morning briefing
   - Agent presents top priorities and focus areas
   - User sets priorities for the day

2. **Work Phase**:
   - User works on tasks
   - Uses `/update` for mid-session checkpoints
   - Agent tracks progress
   - Context is maintained throughout

3. **End of Day**:
   - User runs `/end`
   - Session is summarized
   - Session log is appended
   - State files are updated
   - Optional: `/commit` for git workflow

### Context Loading

At session start, MARVIN loads:
- `state/current.md` - Current priorities, open threads
- `state/goals.md` - Active objectives and progress
- `sessions/YYYY-MM-DD.md` (if resuming) - Previous session logs
- `content/log.md` (if exists) - Recently shipped content

### Context Synthesis

The @briefing subagent combines loaded context into a concise morning briefing with:
- Date and day of week
- Top 3 priorities
- Goal progress
- Active threads
- Suggested focus areas

---

## Tools

MARVIN agents have access to the following tools:

**Read Tools**:
- File reading
- Bash commands (grep, find, ls, pwd, cat)
- Git operations (git log, git diff, git status)

**Write Tools** (with confirmation):
- File creation and modification
- Bash commands
- Web fetching

**Other Tools**:
- `skill` - Load and use MARVIN skills
- `@executor` - Invoke implementation subagent
- `@briefing` - Invoke briefing subagent

**Tool Usage**:
Agents check permissions before each operation. High-risk operations (file edits, git push, emails) always require explicit user confirmation.

---

## Troubleshooting

### Common Issues

**Issue**: Agent not responding
- **Solution**: Check agent assignment, verify model is available

**Issue**: Skill not loading
- **Solution**: Check skill directory path, verify YAML frontmatter

**Issue**: State files not accessible
- **Solution**: Verify file paths, check permissions

**Issue**: Session not saving
- **Solution**: Check sessions/ directory write permissions, verify file naming

**Issue**: Integration not working
- **Solution**: Verify MCP server configuration, check opencode.json

**See AGENTS.md** for complete troubleshooting guide.

---

## Best Practices

### For Agents

1. **Use appropriate agent for task type**
   - General work → marvin
   - Analysis → planner
   - Implementation → executor
   - Briefing → @briefing subagent

2. **Switch agents proactively**
   - Suggest planner when analysis is needed
   - Suggest executor for complex tasks

3. **Keep context loaded**
   - State files should be loaded before decisions
   - Refresh context with `/update` if needed

### For Users

1. **Use `/marvin` to start each session**
   - Loads full context
   - Provides morning briefing
   - Sets up day's priorities

2. **Use `/update` frequently during long sessions**
   - Preserves progress without ending session
   - Creates checkpoint in session log

3. **Use `/end` to close your day**
   - Saves all progress
   - Updates state files
   - Prevents data loss

4. **Use `/report` weekly**
   - Review goal progress
   - Celebrate achievements
   - Plan upcoming week

### For State Management

1. **Keep state/current.md focused**
   - Priorities first
   - Active threads next
   - Remove completed items

2. **Update state/goals.md regularly**
   - Track progress on annual/quarterly objectives
   - Mark milestones as achieved

3. **Let sessions/ be your record**
   - Session logs are append-only
   - Rich history for continuity

---

## File Structure Reference

### MARVIN Directory Structure

```
marvin-template/
├── .opencode/                 [OpenCode configuration]
│   ├── opencode.json        [Main config file]
│   ├── prompts/               [Agent system prompts]
│   │   ├── marvin.txt         [Main agent]
│   │   ├── planner.txt        [Analysis agent]
│   │   ├── executor.txt        [Implementation agent]
│   │   ├── briefing.txt        [Briefing agent]
│   │   └── safety.txt          [Safety guidelines]
│   ├── commands/              [Slash commands]
│   │   ├── marvin.md          [Start session]
│   │   ├── end.md             [End session]
│   │   ├── update.md           [Checkpoint]
│   │   ├── report.md           [Weekly report]
│   │   ├── commit.md           [Git workflow]
│   │   └── sync.md             [Template sync]
│   ├── skills/                 [Agent skills]
│   │   ├── marvin/            [Session management]
│   │   ├── end/                [Session management]
│   │   ├── daily-briefing/    [Morning briefing]
│   │   ├── commit/             [Git operations]
│   │   ├── update/             [Checkpointing]
│   │   ├── report/             [Report generation]
│   │   └── content-shipped/    [Content tracking]
│   ├── tool/                   [Optional custom tools - Phase 9+]
│   ├── themes/                 [Optional themes - Phase 9+]
│   ├── rules/                   [Optional rules - Phase 9+]
│   └── mcp/                    [MCP server config - Phase 9+]
│
├── state/                     [User state files]
│   ├── current.md             [Priorities, threads]
│   ├── goals.md               [Annual/quarterly goals]
│   ├── todos.md               [Task list - optional]
│   └── config/                [User config - optional]
│
├── sessions/                  [Session logs]
│   └── YYYY-MM-DD.md           [Daily sessions]
│
├── content/                   [Content tracking - optional]
│   └── log.md                 [Shipped content]
│
├── reports/                   [Weekly reports]
│   └── YYYY-MM-DD.md           [Weekly summaries]
│
├── docs/                       [Planning and architecture docs]
│   ├── MIGRATION_PLAN.md      [Migration blueprint]
│   ├── AGENTS.md              [Agent system guide]
│   ├── PLAN_OVERVIEW.md       [Quick reference]
│   ├── REFERENCE_ANALYSIS.md   [Reference analysis]
│   ├── ANALYSIS_COMPLETE.md    [Validation summary]
│   ├── PHASE_1_COMPLETE.md     [Phase 1 completion]
│   ├── PHASE_2_COMPLETE.md     [Phase 2 completion]
│   ├── PHASE_3_COMPLETE.md     [Phase 3 completion]
│   ├── OPENCODE.md            [This file - System instructions]
│   ├── INSTALLATION.md         [Setup guide]
│   └── ARCHITECTURE.md         [Technical reference]
│
└── README.md                   [Project documentation]
```

---

## Getting Started

### Installation

1. **Install OpenCode CLI**
   ```bash
   curl -fsSL https://opencode.ai/install | bash
   ```

2. **Clone MARVIN Template**
   ```bash
   git clone https://github.com/anomalyco/marvin-template
   cd marvin-template
   ```

3. **Configure API Key**
   - Run `opencode /connect`
   - Select OpenCode Zen or your preferred provider
   - Enter your API key

4. **First Session**
   ```bash
   cd marvin-template
   opencode
   /marvin
   ```

See **INSTALLATION.md** for detailed setup instructions.

---

## Version History

- **Version 1.0.0** - Initial Claude Code edition
- **Version 1.0.0 (OpenCode)** - Migrated to OpenCode CLI/TUI
  - Complete rewrite of configuration system
  - Enhanced agent architecture (4 agents vs 2)
  - Improved permission system (explicit allow/ask/deny)
  - Native subagent support (executor, briefing)
  - All documentation converted to OpenCode format

---

## Contributing

MARVIN is designed to be extensible. Users can:
- Add custom commands in `.opencode/commands/`
- Add custom skills in `.opencode/skills/`
- Modify agent prompts in `.opencode/prompts/`
- Customize permissions in `.opencode/opencode.json`
- Add custom tools in `.opencode/tool/` (Phase 9+)

See **AGENTS.md** for customization guide and **ARCHITECTURE.md** for extension architecture.

---

## Support

For issues, questions, or feature requests:
- Open an issue on GitHub: https://github.com/anomalyco/marvin-template/issues
- Join the OpenCode Discord: https://opencode.ai/discord
- Join the MARVIN discussions (if available)

---

**Last Updated**: January 28, 2026  
**Maintained by**: MARVIN OpenCode Edition Contributors
