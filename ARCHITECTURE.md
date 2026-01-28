# MARVIN Architecture Reference

This document describes the technical structure of MARVIN and how it integrates with OpenCode.

## Directory Structure

```
marvin-template/
├── .opencode/                    # OpenCode configuration (primary)
│   ├── opencode.json            # OpenCode configuration and agent definitions
│   ├── prompts/                 # Agent system prompts
│   │   ├── marvin.txt          # Main MARVIN chief of staff instructions
│   │   ├── planner.txt         # Analysis and planning mode (read-only)
│   │   ├── executor.txt        # Execution specialist mode
│   │   ├── briefing.txt        # Session briefing generator
│   │   └── safety.txt          # Safety guidelines and constraints
│   ├── commands/               # Command templates
│   │   ├── marvin.md           # Start session with briefing (/marvin)
│   │   ├── end.md              # End session and save (/end)
│   │   ├── update.md           # Checkpoint session (/update)
│   │   ├── report.md           # Weekly report (/report)
│   │   ├── commit.md           # Git commit workflow (/commit)
│   │   └── sync.md             # Template sync (/sync)
│   └── skills/                 # Reusable skill modules
│       ├── marvin/             # Session initialization skill
│       ├── end/                # Session closure skill
│       ├── daily-briefing/     # Briefing generation skill
│       ├── update/             # Checkpoint skill
│       ├── commit/             # Git commit skill
│       ├── report/             # Report generation skill
│       ├── content-shipped/    # Content tracking skill
│       └── _template/          # Skill template for new skills
│
├── state/                       # Persistent session state (kept at root)
│   ├── current.md             # Current priorities and focus
│   ├── goals.md               # Active goals and objectives
│   └── todos.md               # Todo items and tasks
│
├── sessions/                    # Session history logs (append-only)
│   └── YYYY-MM-DD.md          # Daily session entries
│
├── content/                     # Content tracking
│   └── shipped.md             # Published content log
│
├── reports/                     # Weekly report storage
│   └── YYYY-W##-report.md    # Weekly synthesis reports
│
├── skills/                      # Legacy skills location (for reference)
│   └── [deprecated - see .opencode/skills/]
│
└── Documentation files
    ├── OPENCODE.md            # MARVIN system documentation
    ├── INSTALLATION.md        # Setup and installation guide
    ├── ARCHITECTURE.md        # This file
    ├── MIGRATION_PLAN.md      # Migration from Claude to OpenCode
    └── AGENTS.md              # Agent system reference
```

## Core Components

### 1. OpenCode Configuration (.opencode/opencode.json)

The main configuration file that defines:
- **Models**: Which AI models to use (default: zen/big-pickle)
- **Agents**: Four agents with different capabilities and temperatures
- **Commands**: Six command templates that users invoke
- **Permissions**: Safety constraints and approval requirements

**Key Agents**:
- **marvin**: Main agent (temperature 0.3, all tools enabled)
- **planner**: Analysis agent (temperature 0.1, read-only)
- **executor**: Execution specialist (temperature 0.3, subagent)
- **briefing**: Briefing generator (temperature 0.1, subagent)

### 2. Prompt Files (.opencode/prompts/)

System instructions for each agent:
- **marvin.txt**: Core MARVIN personality and role
- **planner.txt**: Analytical, read-only mode instructions
- **executor.txt**: Focused implementation instructions
- **briefing.txt**: Context synthesis and briefing generation
- **safety.txt**: Safety guidelines and constraints

### 3. Commands (.opencode/commands/)

Templates for user commands that invoke agents:

| Command | Function |
|---------|----------|
| `/marvin` | Start session, load context, generate briefing |
| `/end` | End session, save progress, summarize |
| `/update` | Quick checkpoint during session |
| `/report` | Generate weekly synthesis report |
| `/commit` | Git workflow with safety prompts |
| `/sync` | Sync with template updates |

### 4. Skills (.opencode/skills/)

Reusable skill modules that agents can use. Each skill is a markdown file with:
- YAML frontmatter (name, description, compatibility)
- Step-by-step instructions
- Constraints and capabilities

**Included Skills**:
- **marvin**: Session initialization
- **end**: Session closure and state saving
- **daily-briefing**: Context and briefing generation
- **update**: Session checkpointing
- **commit**: Git operations
- **report**: Report generation
- **content-shipped**: Content tracking
- **_template**: Template for creating new skills

### 5. State Files (state/ at root)

Persistent files that survive across sessions:

- **current.md**: Top priorities, focus areas, active projects
- **goals.md**: Long-term goals and milestones
- **todos.md**: Tasks and todo items

These files are:
- Updated by agents during sessions
- Loaded fresh at session start
- Used for context and briefing generation
- Tracked in git for history

### 6. Session Logs (sessions/)

Daily session logs that record:
- What was accomplished
- Decisions made
- Context for future sessions
- Progress on goals

Format: `sessions/YYYY-MM-DD.md` with entries appended chronologically.

## How Commands Work

### Command Execution Flow

1. **User invokes command**: `/marvin` in OpenCode CLI
2. **OpenCode reads**: `.opencode/opencode.json` command definition
3. **Gets template**: `.opencode/commands/marvin.md`
4. **Parses frontmatter**: description, agent assignment
5. **Sends to agent**: Prompt body with loaded context
6. **Agent executes**: Uses available tools (read, write, bash, webfetch)
7. **Output displays**: In OpenCode TUI interface

### Command Example: `/marvin`

```markdown
---
description: Start MARVIN session with briefing
agent: marvin
template: "{file:./.opencode/commands/marvin.md}"
---

Load your MARVIN context and generate a briefing:
1. Load state/current.md and extract top priorities
2. Load state/goals.md for active goals
3. Check today's sessions/YYYY-MM-DD.md for previous context
4. Generate a warm, concise briefing...
```

When invoked:
1. OpenCode parses frontmatter
2. Assigns to `marvin` agent
3. Marvin reads state files
4. Marvin generates briefing
5. Output displays in TUI

## How Skills Work

### Skill Structure

```yaml
---
name: marvin
description: Start MARVIN session with context loading
license: MIT
compatibility: opencode
metadata:
  category: session
  user-invocable: true
---

## What I Do
[Description of capability]

## When to Use
[When this skill should be used]

## Steps
1. [Step 1]
2. [Step 2]
...
```

### Skill Invocation

Skills can be invoked in two ways:

1. **Automatically by agents**: When agent needs a capability
2. **Manually by users**: Via `@skill-name` mention

Example:
```
@executor Please migrate these 5 files using the migration skill
```

## Agent System

### Four-Agent Design

**Primary Agents** (switchable via Tab):
- **MARVIN**: Main interaction agent with full capabilities
- **PLANNER**: Analysis and planning mode (read-only)

**Subagents** (invoked as needed):
- **EXECUTOR**: Fast execution of planned tasks
- **BRIEFING**: Context synthesis and briefing generation

### Temperature Settings

- **MARVIN (0.3)**: Balanced, reliable, slightly creative
- **PLANNER (0.1)**: Highly deterministic, focused analysis
- **EXECUTOR (0.3)**: Reliable implementation
- **BRIEFING (0.1)**: Factual, consistent summaries

### Permission System

Each agent has specific permissions:

```
marvin:
  write: yes (ask)
  bash: yes (allow, ask for git push/rm)
  webfetch: yes (allow)

planner:
  write: no (deny)
  bash: limited (only grep, find, git log/diff)
  webfetch: yes

executor:
  write: yes (allow)
  bash: yes (ask for git push/rm)
  webfetch: yes
```

## State Management

### State File Contents

**state/current.md**:
```markdown
# Current MARVIN State

## Top Priorities
1. [Priority 1]
2. [Priority 2]
3. [Priority 3]

## Active Projects
- [Project A]
- [Project B]

## Next Focus
[What to work on next]
```

**state/goals.md**:
```markdown
# MARVIN Goals

## Q1 Goals
- [Goal 1]
- [Goal 2]

## Long-term Objectives
- [Objective 1]
```

### State Persistence

- Commands read state at session start
- Agents can update state during execution
- Changes committed to git
- History available via `git log state/`

## Integration Points

### OpenCode Integration

MARVIN runs entirely within OpenCode CLI:
- No VSCode plugin needed
- TUI-native interface
- Full CLI tool access (bash, git, npm, etc.)

### MCP Server Integration

OpenCode supports Model Context Protocol servers for:
- External tool access (Slack, Gmail, etc.)
- Web search and fetching
- Database connections
- Custom integrations

Users configure MCP servers in their OpenCode global config.

### Git Integration

MARVIN is fully git-aware:
- Session logs are committed
- State changes are committed
- `/commit` command for structured commits
- History preserved in git log

## Extension Points

### Adding New Skills

1. Create directory: `.opencode/skills/my-skill/`
2. Create `SKILL.md` with YAML frontmatter
3. Add steps and instructions
4. Skill becomes available to all agents

### Adding New Commands

1. Create file: `.opencode/commands/my-command.md`
2. Add frontmatter (description, agent)
3. Add prompt instructions
4. Add to `opencode.json` command definitions

### Customizing Agent Prompts

1. Edit `.opencode/prompts/[agent-name].txt`
2. Adjust tone, style, and instructions
3. Changes take effect on next agent invocation

## Architecture Decisions

### Why This Structure?

1. **Separation of Concerns**: Config, prompts, commands, skills are separate
2. **Modularity**: Skills can be reused across agents
3. **Git-Friendly**: Everything is text, version-controlled
4. **User-Controllable**: Easy to customize and extend
5. **State at Root**: Session state persists independent of tool

### Configuration Philosophy

- **opencode.json**: Tool configuration, not content
- **Prompt files**: Agent behavior, not session data
- **Command templates**: User interactions, not execution
- **Skills**: Reusable patterns and procedures
- **State/**: User data and progress

## Safety Constraints

### Permission Model

1. **Ask-before-destructive**: git push, rm operations require approval
2. **Read-only mode**: PLANNER agent cannot modify files
3. **Limited bash access**: Some agents restrict command types
4. **No force-push**: git force-push is always denied

### Workflow Safety

- `/commit` command includes safety prompts
- MARVIN asks before git push
- Destructive operations require explicit confirmation
- Session logs create audit trail

## Troubleshooting

### Common Issues

**Issue**: Agent can't read state files
- Check: bash permissions in agent config
- Fix: Add grep/find to allowed commands

**Issue**: Commands not triggering
- Check: Frontmatter format (YAML only)
- Fix: Verify agent exists in opencode.json

**Issue**: Skills not discoverable
- Check: SKILL.md filename case
- Fix: Ensure yaml frontmatter is valid

**Issue**: Permissions blocking operations
- Check: `.opencode/opencode.json` permission rules
- Fix: Adjust permission levels as needed

## Performance Considerations

- **State files**: Keep relatively small (<1000 lines)
- **Session logs**: Archive old sessions periodically
- **Skills**: Load on-demand, no performance impact
- **Config**: Loaded once at startup

## Future Extensions

Planned enhancements:
- Custom tools (TypeScript-based)
- Rules system (conditional automation)
- Themes system (UI customization)
- Advanced keybindings
- Full MCP configuration support

See MIGRATION_PLAN.md Phase 9+ for details.

---

## See Also

- `OPENCODE.md` - System documentation and principles
- `INSTALLATION.md` - Setup and first-time configuration
- `AGENTS.md` - Agent system and switching documentation
- `MIGRATION_PLAN.md` - Migration from Claude Code
