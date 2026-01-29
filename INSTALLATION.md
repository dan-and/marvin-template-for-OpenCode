# MARVIN on OpenCode - Installation Guide

**Version**: 1.0.0 (OpenCode Edition)  
**Date**: January 28, 2026  
**Compatibility**: OpenCode CLI/TUI

---

## Overview

MARVIN is an AI Chief of Staff system designed to help you work more efficiently. This guide will walk you through installing MARVIN for OpenCode, setting up your environment, and getting started with your first session.

**What You Need**:
- **OpenCode CLI/TUI** installed (see [Installation](#installation))
- **git** (for managing your repository)
- **Your favorite terminal** (for running OpenCode)

**What You Get**:
- Complete MARVIN AI system with 4 specialized agents
- 6 slash commands for session management
- 8 modular skills for extended functionality
- OpenCode's MCP integration for external tools
- State management and goal tracking

**Time to Setup**: 5 minutes

---

## Quick Start

### 1. Install OpenCode

```bash
# macOS or Linux
curl -fsSL https://opencode.ai/install | bash

# Using Homebrew (recommended)
brew install anomalyco/tap/opencode

# Using NPM
npm i -g opencode-ai@latest

# Windows
winget install Anthropic.ClaudeCode
```

**Verify Installation**:
```bash
opencode version
```

Should show `opencode-ai/[version]`

### 2. Clone MARVIN Template

```bash
git clone https://github.com/dan-and/marvin-template-for-OpenCode.git
cd marvin-template-for-OpenCode
```

**Note**: This OpenCode edition is based on the original [MARVIN template for Claude Code](https://github.com/SterlingChin/marvin-template) by Sterling Chin.

### 3. Configure API Key

```bash
# If using OpenCode Zen model (recommended)
# No API key needed - zen/big-pickle is always available

# If using other providers (Anthropic, OpenAI, etc.)
# Set environment variable or configure in opencode.json

# Example (Anthropic)
export ANTHROPIC_API_KEY="your-api-key-here"
```

### 4. Start Your First Session

```bash
cd marvin-template
opencode
```

OpenCode TUI will load the configuration and present the MARVIN agent.

**Type**: `/marvin` (Start session with briefing)

Your first session will:
- Load your current priorities and goals
- Check today's session log
- Generate a personalized morning briefing
- Set context for productive work

---

## Configuration

### OpenCode Config

MARVIN's configuration is in `.opencode/opencode.json`. The key settings are:

**Models**:
- Default: `zen/big-pickle` (OpenCode's default model, always available)
- Can be changed to other models in config

**Agents**:
- **marvin** (Primary, 0.3°) - Full capabilities for general work
- **planner** (Primary, 0.1°) - Read-only analysis without changes
- **executor** (Subagent, 0.3°) - Fast implementation
- **briefing** (Subagent, 0.1°) - Session briefing

**Permission System** (3-level: Allow/Ask/Deny):
- **Allow**: Read operations (grep, find, git log, ls), webfetch
- **Ask**: File modifications, git push, email/messages
- **Deny**: git force-push, unsafe deletions

**Commands** (6 slash commands):
- `/marvin` - Start session with briefing
- `/end` - End session and save progress
- `/update` - Quick checkpoint
- `/report` - Weekly summary
- `/commit` - Git workflow
- `/sync` - Template updates

**Skills** (8 modular capabilities):
- Session management
- Briefing generation
- Git workflow
- Content tracking
- And more!

### Custom Configuration

You can customize MARVIN by editing `.opencode/opencode.json`:

```json
{
  "models": {
    "default": "your-preferred-model"
  }
}
```

---

## State Management

MARVIN uses these state files to maintain continuity:

### State Files

```
marvin-template/
├── .opencode/              [OpenCode configuration]
├── state/                 [Your personal data]
│   ├── current.md         [Active priorities, open threads]
│   ├── goals.md           [Your objectives]
│   └── todos.md           [Task list - optional]
├── sessions/               [Daily session logs]
│   └── YYYY-MM-DD.md     [Per day]
├── content/               [Content tracking - optional]
│   └── log.md             [Shipped content log]
└── reports/               [Weekly reports - optional]
    └── YYYY-MM-DD.md     [Weekly summaries]
```

### Initial Setup

On first use, MARVIN will create session files if they don't exist:

1. **state/current.md** - Template for your priorities
2. **state/goals.md** - Template for your objectives
3. **state/todos.md** - Template for your task list (optional)

These templates will be in the repository, so MARVIN starts with structure immediately.

---

## Basic Commands

### Session Management

#### /marvin
Start a new MARVIN session with briefing.

**What it does**:
- Loads your current priorities and goals
- Checks today's session log
- Generates personalized morning briefing
- Sets context for productive work

**When to use**:
- At the start of your day
- When resuming work after a break
- When starting a new project

#### /end
End your current MARVIN session.

**What it does**:
- Summarizes conversation
- Updates state files
- Saves to session log
- Optionally commits changes

**When to use**:
- When you're done for the day
- When closing out of your workspace
- Before leaving for a while

#### /update
Quick checkpoint during session.

**What it does**:
- Saves progress to session log
- No state file updates (lightweight)
- Preserves context mid-session

**When to use**:
- After completing a significant piece of work
- Before switching contexts
- Every hour or so during long sessions

#### /report
Generate weekly summary report.

**What it does**:
- Synthesizes work across all sessions from the past week
- Reviews goal progress
- Lists shipped content
- Identifies blockers and issues
- Provides next steps

**When to use**:
- At the end of each week
- Before starting a new week

#### /commit
Review and commit git changes.

**What it does**:
- Groups changes logically
- Creates focused commits
- Uses conventional commit messages
- Supports safe git workflow

**When to use**:
- After completing a feature or task
- At the end of a work session
- Before pushing to remote

#### /sync
Sync with MARVIN template updates.

**What it does**:
- Checks for updates from MARVIN template repository
- Applies only safe, non-destructive changes
- Preserves all your personal data (state/, sessions/)
- Always asks for permission before any modifications

**When to use**:
- Occasionally to get new features
- When project structure changes significantly
- Monthly or quarterly to stay up to date

---

## Skills

MARVIN includes 8 skills that extend functionality:

### Core Skills

**marvin** (Session Start)
- Automatic session initialization
- Context loading and briefing generation

**end** (Session End)
- Session summarization and state saving
- Session log management

**daily-briefing** (Morning Briefing)
- Daily context loading and briefing
- Progress assessment
- Alert generation

**commit** (Git Workflow)
- Change grouping and focused commits
- Conventional commit messages
- Safe git workflow

**update** (Session Checkpoint)
- Lightweight mid-session saves
- Progress preservation
- Context maintenance

**report** (Weekly Report)
- Session synthesis across all sessions
- Goal progress tracking
- Shipped content and achievements

**content-shipped** (Content Tracking)
- Content logging
- Goal association

### How to Use Skills

Skills are loaded automatically by MARVIN agents. You can also invoke specific skills by name:

- Type the skill name directly in conversation
- MARVIN will find and load the skill
- Skills can also be invoked by agents automatically

---

## Integrations

MARVIN can connect to external services via OpenCode's MCP (Model Context Protocol) system.

### Available Integrations

- **Google Workspace** - Gmail, Calendar, Drive access
- **Microsoft 365** - Outlook, Teams, OneDrive access
- **Atlassian** - Jira, Confluence access
- **Slack/Teams/Discord** - Messaging and collaboration
- **Parallel Search** - Web search and content fetching

### Setting Up

Integrations are configured in OpenCode's global configuration. MARVIN can then access these services through MCP servers.

**Note**: You configure integrations, not MARVIN. MARVIN uses the integrations you've set up in OpenCode.

---

## Agent Switching

MARVIN uses a multi-agent system for different tasks.

### Primary Agents

Switch with the `Tab` key to cycle through primary agents:

- **marvin** (Default) - Full capabilities, general work
- **planner** (Analysis) - Read-only, no changes

### Subagents

Agents that can be invoked directly or called by other agents:

- **executor** - Fast implementation of planned tasks
- **briefing** - Session briefing generation

### How to Use Agents

1. **Auto Switching** - MARVIN may suggest switching agents based on context
2. **Manual Switching** - Press `Tab` to cycle through primary agents
3. **Subagent Invocation** - Type `@executor` or `@briefing` to invoke directly

### Temperature Settings

Each agent uses a specific temperature for optimal performance:

- **marvin**: 0.3° - Balanced, creative but reliable
- **planner**: 0.1° - Highly deterministic, focused
- **executor**: 0.3° - Action-oriented, efficient
- **briefing**: 0.1° - Factual, consistent

---

## Safety

MARVIN is designed to be safe by default. High-risk operations always require explicit confirmation.

### What Requires Confirmation

Before MARVIN executes, you'll see confirmation prompts for:

- **Email/Message Sending** - "I'm about to send an email to [recipient]. Should I proceed?"
- **File Modifications** - "I'm about to delete/overwrite [file]. Should I proceed?"
- **Ticket Creation** - "I'm about to create Jira ticket [key]. Should I proceed?"
- **Git Push** - "I'm about to push changes. Should I proceed?"
- **Publishing Content** - "I'm about to publish [content]. Should I proceed?"

### What Doesn't Require Confirmation

- Reading files
- Searching code (grep, find)
- Git status checks
- Loading context
- Viewing session logs
- Web fetching research

### Denied Operations

MARVIN will refuse to execute:

- git force-push (can't recover)
- rm without safeguards
- Unsafe deletions

---

## Troubleshooting

### Common Issues

**MARVIN not responding**:
- Check that OpenCode is running
- Try restarting OpenCode: `opencode` (quit and start again)

**Configuration not loading**:
- Verify `.opencode/opencode.json` is valid JSON
- Check that all paths in config are correct

**Skills not loading**:
- Check that skill files exist in `.opencode/skills/`
- Verify YAML frontmatter is correct

**State files not accessible**:
- Verify permissions on `state/` directory
- Check that files are readable

### Getting Help

- Join the OpenCode Discord: https://opencode.ai/discord
- Check the documentation: `cat MARVIN.md`
- Review `AGENTS.md` for agent system details
- See the migration plan: `cat MIGRATION_PLAN.md`

---

## Next Steps

1. **Install OpenCode** (5 minutes)
2. **Clone MARVIN template** (2 minutes)
3. **Start your first session** (1 minute)
   ```bash
   cd marvin-template
   opencode
   ```
   Type `/marvin`

4. **Get to work!**

MARVIN will load your context, present a briefing, and help you have a productive day.

---

## Support

For issues, questions, or feature requests:

- **GitHub Issues**: https://github.com/dan-and/marvin-template-for-OpenCode/issues

**Original Template**: This project is based on the original [MARVIN template for Claude Code](https://github.com/SterlingChin/marvin-template) by Sterling Chin.
- **OpenCode Discord**: https://opencode.ai/discord
- **Documentation**: See `MARVIN.md` for complete system reference

---

**Welcome to MARVIN on OpenCode!** 🚀

Your AI Chief of Staff is ready to help you work more efficiently. Let's get started!
