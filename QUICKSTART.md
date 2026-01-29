# MARVIN Quick Start Guide

Get up and running with MARVIN in 10 minutes.

---

## Prerequisites

- OpenCode CLI installed (https://opencode.ai)
- Basic command line familiarity
- Git (for version control)

---

## Step 1: Clone the Template (2 minutes)

```bash
git clone https://github.com/dan-and/marvin-template-for-OpenCode.git ~/marvin
cd ~/marvin
```

**Note**: This OpenCode edition is based on the original [MARVIN template for Claude Code](https://github.com/SterlingChin/marvin-template) by Sterling Chin.

---

## Step 2: Verify Configuration (1 minute)

Check that OpenCode recognizes the configuration:

```bash
opencode --config .opencode/opencode.json
```

Expected output: Configuration loads without errors.

---

## Step 3: Initialize State Files (1 minute)

Update the state files with your information:

**state/current.md** - Add your top 3 priorities:
```markdown
# Current MARVIN State

## Top Priorities
1. My first priority
2. My second priority
3. My third priority

## Active Projects
- Project A
- Project B

## Next Focus
What I want to work on next
```

**state/goals.md** - Add your goals:
```markdown
# MARVIN Goals

## Q1 Goals
- Goal 1
- Goal 2

## Long-term Objectives
- Objective 1
```

---

## Step 4: Start Your First Session (2 minutes)

Open OpenCode and run:

```bash
/marvin
```

This will:
- Load your context from state files
- Generate a personalized briefing
- Show today's priorities
- List active goals

---

## Step 5: Work and Checkpoint (3 minutes)

During your session, work on tasks. Every so often, checkpoint:

```bash
/update
```

This saves your progress to the session log.

---

## Step 6: End Your Session (1 minute)

When you're done:

```bash
/end
```

This will:
- Ask what you accomplished
- Summarize your session
- Save everything to sessions/YYYY-MM-DD.md
- Update your state files

---

## Your First Commands

### `/marvin` - Start Session
Loads context and generates briefing.
```bash
/marvin
```

### `/update` - Checkpoint
Saves progress during session.
```bash
/update
```

### `/report` - Weekly Report
Generates synthesis of work from the past week.
```bash
/report
```

### `/commit` - Git Workflow
Commits changes with safety confirmation.
```bash
/commit
```

### `/end` - End Session
Summarizes and saves session.
```bash
/end
```

### `/sync` - Sync Template
Updates template with latest changes (asks for confirmation).
```bash
/sync
```

---

## Switch Agents

During your session, press **Tab** to switch between agents:

- **MARVIN** (Default) - Full capabilities, all tools enabled
- **PLANNER** (Analysis) - Read-only mode, perfect for planning

Example workflow:
1. Start with MARVIN for full capabilities
2. Press Tab to switch to PLANNER for analysis
3. Press Tab again to switch back to MARVIN

---

## File Structure

Your MARVIN workspace has this structure:

```
marvin/
├── .opencode/              # OpenCode configuration
│   ├── opencode.json      # Agent and command definitions
│   ├── prompts/           # Agent system instructions
│   ├── commands/          # Command templates
│   └── skills/            # Reusable skill modules
│
├── state/                 # Your persistent state
│   ├── current.md        # Current priorities
│   ├── goals.md          # Active goals
│   └── todos.md          # Task tracking
│
├── sessions/             # Session logs (created automatically)
├── reports/              # Weekly reports (created automatically)
├── content/              # Content tracking
│
└── Documentation files
    ├── OPENCODE.md       # System documentation
    ├── INSTALLATION.md   # Detailed setup guide
    ├── ARCHITECTURE.md   # Technical reference
    └── AGENTS.md         # Agent system guide
```

---

## Common Workflows

### Daily Workflow

```bash
# Morning: Start session
/marvin

# During day: Checkpoint progress
/update

# End of day: Save session
/end
```

### Weekly Workflow

```bash
# Friday afternoon
/report

# Review weekly summary
# Update goals and priorities for next week
```

### When Making Changes

```bash
# Make changes to files
vim state/current.md

# Commit changes
/commit

# This asks for your commit message and confirmation
```

### When Analyzing

```bash
# Press Tab to switch to PLANNER agent (read-only analysis mode)
Tab

# Ask questions like:
# "What should I prioritize?"
# "Is this approach viable?"
# "What's my progress toward goals?"

# Press Tab again to switch back to MARVIN when ready to execute
Tab
```

---

## Tips & Best Practices

### 1. Keep State Files Updated
Update `state/current.md` regularly so briefings reflect current reality.

### 2. Use Checkpoints
`/update` frequently during long sessions to save progress incrementally.

### 3. Weekly Reports
Run `/report` every Friday to synthesize your week's work.

### 4. Use PLANNER Agent
Press Tab to switch to PLANNER for analysis without the risk of making changes.

### 5. Git Integration
Every session is logged in git. You can review history:
```bash
git log --oneline state/
git log --oneline sessions/
```

### 6. Read the Docs
- Quick reference: This file
- Detailed setup: INSTALLATION.md
- System guide: OPENCODE.md
- Technical details: ARCHITECTURE.md

---

## Troubleshooting

### Commands Not Working

**Problem**: `/marvin` or other commands not recognized

**Solution**: 
1. Make sure you're in the marvin directory: `cd ~/marvin`
2. Verify OpenCode is running: `opencode`
3. Check config: `.opencode/opencode.json` should exist

### Agent Not Responding

**Problem**: Agent seems stuck or unresponsive

**Solution**:
1. Try a simpler command like `/update`
2. Check agent status (Tab key should show current agent)
3. Exit and restart: `exit`, then `opencode` again

### State Files Not Updating

**Problem**: Changes to state files don't appear in briefings

**Solution**:
1. Make sure files are saved: `:w` in editor
2. Exit MARVIN and start new session: `/end`, then `/marvin`
3. Check file paths are correct in state/ directory

### Git Errors

**Problem**: `/commit` says permission denied or other git error

**Solution**:
1. Check you have git credentials: `git config --global user.name`
2. Make sure you're in a git repository: `git status`
3. Try again: `/commit`

---

## Next Steps

1. **Customize Your Setup**
   - Edit state files with your goals and priorities
   - Read INSTALLATION.md for API configuration

2. **Learn the System**
   - Read OPENCODE.md for complete system documentation
   - Check AGENTS.md to understand agent system

3. **Explore Features**
   - Try all 6 commands
   - Switch between agents with Tab key
   - Review your session logs

4. **Read Full Documentation**
   - INSTALLATION.md - Complete setup guide
   - ARCHITECTURE.md - Technical architecture
   - TESTING.md - Test procedures

---

## Key Concepts

**Agent**: AI assistant with specific capabilities and constraints
- MARVIN: Main agent with all capabilities
- PLANNER: Read-only analysis agent

**Command**: User action that invokes MARVIN (like `/marvin`, `/end`)

**Skill**: Reusable capability that agents can use

**State**: Your persistent priorities, goals, and todos

**Session**: One period of work (start with `/marvin`, end with `/end`)

**Briefing**: Summary of your context, priorities, and progress

---

## Getting Help

**For Issues**:
1. Check OPENCODE.md troubleshooting section
2. Review AGENTS.md for agent-specific help
3. Report issues at: https://github.com/anomalyco/opencode

**For Questions**:
1. Read ARCHITECTURE.md for technical questions
2. Read INSTALLATION.md for setup questions
3. Check AGENTS.md for agent-related questions

---

## What's Next?

Once you're comfortable with the basics:

1. **Customize Agents**: Edit `.opencode/prompts/marvin.txt` to adjust MARVIN's personality
2. **Create Skills**: Add new skills in `.opencode/skills/[name]/SKILL.md`
3. **Add Commands**: Define new commands in `.opencode/opencode.json`
4. **Configure Integrations**: Set up MCP servers for external tools

See ARCHITECTURE.md for extension points and customization.

---

## Support & Feedback

- **OpenCode Docs**: https://opencode.ai/docs
- **GitHub Issues**: https://github.com/anomalyco/opencode
- **This Template**: https://github.com/dan-and/marvin-template-for-OpenCode

**Original Template**: This project is based on the original [MARVIN template for Claude Code](https://github.com/SterlingChin/marvin-template) by Sterling Chin.

Enjoy using MARVIN! 🚀
