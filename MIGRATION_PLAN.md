# MARVIN → OpenCode: FINAL EXECUTION BLUEPRINT
## Complete Implementation Plan - Ready to Execute

**Status**: PLAN MODE - Ready for Implementation  
**Answers Confirmed**:
- ✅ Q1: Keep `state/` at root
- ✅ Q2: Delete `.marvin/integrations/`, use OpenCode MCP system
- ✅ Q3: Remove all Claude Code support entirely
- ✅ Q4: Ask user permission before syncing (GitHub URL may change)
- ✅ Q5: Use `zen/big-pickle` as default model

**Scope**: Full OpenCode migration, zero Claude dependencies  
**Duration**: 3-4 weeks (part-time development)  
**Complexity**: Medium (straightforward migrations, no complex logic changes)

---

## REFERENCE ANALYSIS UPDATE

**Analysis Date**: January 28, 2026

During reference analysis of OpenCode capabilities, the following features were identified but are **optional for Phase 1**:

### Optional Features (Documented but Not Implemented Yet)

1. **Custom Tools** (`.opencode/tool/`)
   - TypeScript-based tool implementations
   - More powerful than skills for complex operations
   - Not needed for core MARVIN functionality
   - Can be added in Phase 9+ for advanced features

2. **Themes System** (`.opencode/themes/`)
   - JSON-based UI themes
   - Optional branding opportunity
   - Not required for functionality
   - Can be added later for visual customization

3. **Rules System** (Conditional triggering)
   - Smart automation based on conditions
   - Advanced feature for intelligent suggestions
   - Can be explored in Phase 9+

4. **Keybinding Customization**
   - Available in opencode.jsonc
   - Users can customize key sequences
   - Documented in OPENCODE.md

5. **Full MCP Configuration Support**
   - OpenCode has native MCP server registration
   - Users configure their own MCP servers
   - Documentation references in OPENCODE.md

**Impact on Phase 1**: ✅ NONE - These are optional enhancements

**Recommendation**: Proceed with current Phase 1 plan. Optional features can be added in Phase 9+ or as user requests.

For detailed analysis, see **REFERENCE_ANALYSIS.md**

---

## PART 1: CONFIGURATION BLUEPRINT

### 1.1 - Complete opencode.json

```json
{
  "$schema": "https://opencode.ai/config.json",

  "models": {
    "default": "zen/big-pickle",
    "planning": "zen/big-pickle",
    "execution": "zen/big-pickle"
  },

  "agent": {
    "marvin": {
      "description": "Main MARVIN AI Chief of Staff with full capabilities",
      "mode": "primary",
      "model": "zen/big-pickle",
      "temperature": 0.3,
      "prompt": "{file:./.opencode/prompts/marvin.txt}",
      "tools": {
        "write": true,
        "edit": true,
        "bash": true,
        "webfetch": true,
        "skill": true
      },
      "permission": {
        "edit": "ask",
        "bash": {
          "*": "allow",
          "git push": "ask",
          "git force-push": "deny",
          "rm": "ask"
        },
        "webfetch": "allow"
      }
    },

    "planner": {
      "description": "Analysis and planning - read-only, no changes",
      "mode": "primary",
      "model": "zen/big-pickle",
      "temperature": 0.1,
      "prompt": "{file:./.opencode/prompts/planner.txt}",
      "tools": {
        "write": false,
        "edit": false,
        "bash": {
          "grep": "allow",
          "find": "allow",
          "git log": "allow",
          "git diff": "allow",
          "ls": "allow",
          "pwd": "allow",
          "*": "deny"
        },
        "webfetch": true,
        "skill": true
      },
      "permission": {
        "edit": "deny",
        "bash": {
          "*": "deny",
          "grep *": "allow",
          "find *": "allow",
          "git log*": "allow",
          "git diff*": "allow"
        }
      }
    },

    "executor": {
      "description": "Execution specialist - focused implementation",
      "mode": "subagent",
      "model": "zen/big-pickle",
      "temperature": 0.3,
      "prompt": "{file:./.opencode/prompts/executor.txt}",
      "tools": {
        "write": true,
        "edit": true,
        "bash": true,
        "webfetch": true,
        "skill": true
      },
      "permission": {
        "edit": "allow",
        "bash": {
          "git push": "ask",
          "git force-push": "deny",
          "rm": "ask"
        }
      }
    },

    "briefing": {
      "description": "Session briefing specialist",
      "mode": "subagent",
      "model": "zen/big-pickle",
      "temperature": 0.1,
      "prompt": "{file:./.opencode/prompts/briefing.txt}",
      "tools": {
        "write": false,
        "edit": false,
        "bash": {
          "find": "allow",
          "grep": "allow",
          "cat": "allow",
          "ls": "allow",
          "*": "deny"
        },
        "webfetch": false,
        "skill": true
      }
    }
  },

  "command": {
    "marvin": {
      "description": "Start MARVIN session with briefing",
      "agent": "marvin",
      "template": "{file:./.opencode/commands/marvin.md}"
    },
    "end": {
      "description": "End session and save progress",
      "agent": "marvin",
      "template": "{file:./.opencode/commands/end.md}"
    },
    "update": {
      "description": "Quick checkpoint during session",
      "agent": "marvin",
      "template": "{file:./.opencode/commands/update.md}"
    },
    "report": {
      "description": "Generate weekly summary report",
      "agent": "marvin",
      "template": "{file:./.opencode/commands/report.md}"
    },
    "commit": {
      "description": "Review and commit git changes",
      "agent": "marvin",
      "template": "{file:./.opencode/commands/commit.md}"
    },
    "sync": {
      "description": "Sync with MARVIN template updates",
      "agent": "marvin",
      "template": "{file:./.opencode/commands/sync.md}"
    }
  },

  "permission": {
    "edit": "ask",
    "bash": {
      "git *": "ask",
      "rm": "ask",
      "mv": "ask",
      "rm -rf": "deny"
    },
    "webfetch": "allow",
    "skill": {
      "*": "allow"
    }
  },

  // OPTIONAL FEATURES (Phase 9+)
  // Uncomment to enable advanced features

  // Custom Tools (TypeScript-based extensions)
  // "tool": {
  //   // Tools can be added here or in .opencode/tool/
  // },

  // MCP Servers (for users to add their own)
  // "mcp": {
  //   // "custom-server": {
  //   //   "type": "remote",
  //   //   "url": "https://your-mcp-server.com"
  //   // }
  // },

  // Rules System (conditional triggering)
  // "rule": {
  //   // "auto-end-session": {
  //   //   "condition": "time_after('17:00')",
  //   //   "action": "suggest_end"
  //   // }
  // },

  // Keybindings (user customization)
  // "keybind": {
  //   "switch_agent": "Tab"
  // }
}
```

### 1.2 - Prompt Files to Create

**Count**: 5 prompt files  
**Location**: `.opencode/prompts/`

1. **marvin.txt** - Main system instructions (~800 words)
2. **planner.txt** - Analysis mode (~300 words)
3. **executor.txt** - Implementation mode (~200 words)
4. **briefing.txt** - Briefing generation (~250 words)
5. **safety.txt** - Safety guidelines (~200 words) - Reference only, included in marvin.txt

**Total content**: ~1,500 words of system instructions

---

## PART 2: COMMANDS TO MIGRATE

### 2.1 - Command Files Structure

**Location**: `.opencode/commands/` (6 files)

| File | Frontmatter | Template | Triggers |
|------|-------------|----------|----------|
| `marvin.md` | desc, agent | Load state, generate briefing | `/marvin` |
| `end.md` | desc, agent | Summarize, save session | `/end` |
| `update.md` | desc, agent | Checkpoint session | `/update` |
| `report.md` | desc, agent | Weekly synthesis | `/report` |
| `commit.md` | desc, agent | Git workflow | `/commit` |
| `sync.md` | desc, agent | Template sync with confirmation | `/sync` |

### 2.2 - Command File Templates

Each command is a markdown file with:
- YAML frontmatter (description, agent assignment)
- Prompt/instructions that become the user request

**Pattern**:
```markdown
---
description: What this command does
agent: marvin
---

# Command Name

Detailed instructions for what the agent should do when this command is invoked.
```

### 2.3 - Key Migration Differences

| Aspect | Old (Claude) | New (OpenCode) |
|--------|------------|---------------|
| IDE Integration | Claude sidebar reference | TUI-focused output |
| File editing | VSCode integration | Direct file operations |
| Output format | IDE panel styling | Console-friendly markdown |
| Agent selection | Implicit | Via frontmatter `agent:` |

---

## PART 3: SKILLS TO MIGRATE

### 3.1 - Skills Migration Matrix

**Location**: `.opencode/skills/*/SKILL.md` (7 files + 1 template)

| Skill | Current | New Location | Frontmatter Changes |
|-------|---------|--------------|-------------------|
| marvin | `skills/marvin/` | `.opencode/skills/marvin/` | Add `compatibility: opencode` |
| end | `skills/end/` | `.opencode/skills/end/` | Add `compatibility: opencode` |
| daily-briefing | `skills/daily-briefing/` | `.opencode/skills/daily-briefing/` | Add `compatibility: opencode` |
| commit | `skills/commit/` | `.opencode/skills/commit/` | Add `compatibility: opencode` |
| update | `skills/update/` | `.opencode/skills/update/` | Add `compatibility: opencode` |
| report | `skills/report/` | `.opencode/skills/report/` | Add `compatibility: opencode` |
| content-shipped | `skills/content-shipped/` | `.opencode/skills/content-shipped/` | Add `compatibility: opencode` |
| _template | `skills/_template/` | `.opencode/skills/_template/` | Add `compatibility: opencode` |

**NOT migrating**:
- `skills/skill-creator/SKILL.md` - Not needed in OpenCode (users create via `/skill create`)

### 3.2 - Skill Frontmatter Conversion

**Old Format** (Claude):
```yaml
name: marvin
description: Start MARVIN session with briefing
license: MIT
compatibility: marvin
metadata:
  marvin-category: session
  user-invocable: true
  slash-command: /marvin
```

**New Format** (OpenCode):
```yaml
name: marvin
description: Start MARVIN session with briefing and load context
license: MIT
compatibility: opencode
metadata:
  category: session
  user-invocable: true
```

**Key Changes**:
- `compatibility: marvin` → `compatibility: opencode`
- Remove Claude-specific metadata keys
- Keep: name, description, license, basic metadata

---

## PART 4: DIRECTORY STRUCTURE CHANGES

### 4.1 - What to Create

```
.opencode/                          [NEW - Create]
├── opencode.json                   [NEW - Create]
├── prompts/                        [NEW - Create]
│   ├── marvin.txt                 [NEW - Create]
│   ├── planner.txt                [NEW - Create]
│   ├── executor.txt               [NEW - Create]
│   ├── briefing.txt               [NEW - Create]
│   └── safety.txt                 [NEW - Create, reference only]
├── commands/                       [NEW - Create]
│   ├── marvin.md                  [NEW - Migrate from .claude/]
│   ├── end.md                     [NEW - Migrate from .claude/]
│   ├── update.md                  [NEW - Migrate from .claude/]
│   ├── report.md                  [NEW - Migrate from .claude/]
│   ├── commit.md                  [NEW - Migrate from .claude/]
│   └── sync.md                    [NEW - Migrate from .claude/]
└── skills/                         [NEW - Create]
    ├── marvin/
    │   └── SKILL.md               [NEW - Migrate from skills/]
    ├── end/
    │   └── SKILL.md               [NEW - Migrate from skills/]
    ├── daily-briefing/
    │   └── SKILL.md               [NEW - Migrate from skills/]
    ├── commit/
    │   └── SKILL.md               [NEW - Migrate from skills/]
    ├── update/
    │   └── SKILL.md               [NEW - Migrate from skills/]
    ├── report/
    │   └── SKILL.md               [NEW - Migrate from skills/]
    ├── content-shipped/
    │   └── SKILL.md               [NEW - Migrate from skills/]
    └── _template/
        └── SKILL.md               [NEW - Migrate from skills/]
```

### 4.2 - What to Delete

```
DELETE ENTIRELY:
├── .claude/                        [DELETE - all Claude Code files]
├── .marvin/integrations/           [DELETE - Claude-specific integrations]
├── CLAUDE.md                       [DELETE - Claude system file]
└── skills/skill-creator/           [DELETE - not needed in OpenCode]
```

### 4.3 - What to Keep (at root)

```
KEEP AS-IS:
├── state/                          [KEEP - same location]
│   ├── current.md
│   ├── goals.md
│   └── todos.md
├── sessions/                       [KEEP - append mode]
├── content/                        [KEEP - content tracking]
├── reports/                        [KEEP - weekly reports]
└── skills/                         [KEEP - for backward compat reference]
```

---

## PART 5: DOCUMENTATION UPDATES

### 5.1 - Documentation Files

**To Create** (3 new files):

1. **`OPENCODE.md`** (~2,000 words)
   - MARVIN overview and philosophy
   - Core principles and personality
   - Safety guidelines
   - Session flow
   - State management
   - Integration setup
   - Command reference
   - Troubleshooting

2. **`INSTALLATION.md`** (~1,000 words)
   - Prerequisites
   - Install OpenCode
   - Clone MARVIN template
   - First-time setup
   - Configure API keys
   - Run first session
   - Verify setup

3. **`ARCHITECTURE.md`** (~1,500 words)
   - Directory structure
   - How commands work
   - How skills work
   - Agent system
   - State persistence
   - Integration system
   - Extension points

**To Update** (2 files):

4. **`README.md`** (Rewrite major sections)
   - Remove all Claude Code references
   - Lead with OpenCode
   - Update installation instructions
   - Update quick start
   - Link to INSTALLATION.md and OPENCODE.md

5. **`.gitignore`**
   - Remove Claude Code entries
   - Keep: `.env`, `node_modules`, `*.db`, `__pycache__`, etc.
   - Add: `.DS_Store`, build artifacts

---

## PART 6: INTEGRATION HANDLING

### 6.1 - MCP Server Configuration

**Current**: `.marvin/integrations/` with setup scripts  
**New**: Configure in `opencode.json` MCP servers section

**Strategy**:
1. Delete `.marvin/integrations/` directory
2. Create documentation on how to configure each MCP server
3. Users configure their own MCP servers in OpenCode global config
4. MARVIN documentation references OpenCode MCP docs

**Available integrations** (users configure themselves):
- Google Workspace (Gmail, Calendar, Drive)
- Microsoft 365 (Outlook, Teams, OneDrive)
- Atlassian (Jira, Confluence)
- Parallel Search (web search/fetch)
- Slack (messaging)
- Telegram (bot)

**Note**: These are optional - users set up based on their needs

---

## PART 7: EXECUTION SEQUENCE

### Phase 1: Configuration Foundation (3 hours)
- [ ] Create `.opencode/` directory structure
- [ ] Create `opencode.json` with full config
- [ ] Create 5 prompt files (marvin.txt, planner.txt, executor.txt, briefing.txt, safety.txt)
- [ ] **Test**: Verify `opencode` CLI reads config correctly

### Phase 2: Command Migration (2 hours)
- [ ] Create `.opencode/commands/` directory
- [ ] Copy and adapt 6 command files
- [ ] Update frontmatter for OpenCode format
- [ ] **Test**: Run each command (e.g., `/marvin`, `/end`, `/report`)

### Phase 3: Skill Migration (2 hours)
- [ ] Create `.opencode/skills/` directory structure
- [ ] Copy 8 skill directories
- [ ] Update YAML frontmatter in each SKILL.md
- [ ] **Test**: Verify skills load and are discoverable

### Phase 4: Documentation Rewrite (4 hours)
- [ ] Rewrite README.md for OpenCode
- [ ] Create OPENCODE.md (system instructions)
- [ ] Create INSTALLATION.md (setup guide)
- [ ] Create ARCHITECTURE.md (technical reference)
- [ ] Update .gitignore

### Phase 5: Clean Up (1 hour)
- [ ] Delete `.claude/` directory entirely
- [ ] Delete `.marvin/integrations/` directory
- [ ] Delete `CLAUDE.md` file
- [ ] Delete `skills/skill-creator/` directory
- [ ] Verify git status shows only deletions

### Phase 6: Integration Documentation (1 hour)
- [ ] Create reference documentation for MCP setup
- [ ] Update OPENCODE.md with integration instructions
- [ ] Add examples for each integration type

### Phase 7: Final Testing (4 hours)
- [ ] Full workflow test (session start to end)
- [ ] Agent switching test (Tab key)
- [ ] Command execution test (all 6 commands)
- [ ] State persistence test
- [ ] Subagent invocation test (@executor)
- [ ] Safety confirmation test (git push)

### Phase 8: Polish & Documentation (2 hours)
- [ ] Create TROUBLESHOOTING.md
- [ ] Add FAQ section to OPENCODE.md
- [ ] Create quick reference guide
- [ ] Final review of all files

**Total Effort**: 19-21 hours (can be done over 2-3 weeks part-time)

---

## PART 8: CRITICAL IMPLEMENTATION NOTES

### 8.1 - Command Development Pattern

Each command needs:
1. **Frontmatter**: Minimal metadata
2. **Prompt Body**: Clear, actionable instructions
3. **Context Loading**: Reference state files appropriately
4. **Output**: Markdown-formatted for TUI display

**Example: `/marvin` command**
```markdown
---
description: Start MARVIN session with briefing
agent: marvin
---

Load your MARVIN context and generate a briefing:

1. Load state/current.md and extract top priorities
2. Load state/goals.md for active goals
3. Check today's sessions/YYYY-MM-DD.md for previous context
4. Generate a warm, concise briefing with:
   - Today's date and day of week
   - Top 3 priorities
   - Progress toward goals
   - Active open threads
   - Suggested focus areas

Keep the briefing to 3-4 short paragraphs. Be direct but warm.
```

### 8.2 - Skill Development Pattern

Each skill needs:
1. **YAML Frontmatter**: name, description, license, compatibility
2. **Content**: Clear steps and instructions
3. **References**: How to use the skill
4. **Constraints**: What the skill can/cannot do

**Example: `marvin/SKILL.md` header**
```yaml
---
name: marvin
description: Start MARVIN session with context loading and briefing generation
license: MIT
compatibility: opencode
metadata:
  category: session
  user-invocable: true
---

## What I Do
- Load your current priorities and goals
- Analyze today's session history
- Generate a personalized briefing
- Set context for productive work

## When to Use
Use this at the beginning of your day or when starting a new MARVIN session.
```

### 8.3 - Agent Configuration Validation

**Verify after config creation**:
- [ ] `opencode.json` is valid JSON
- [ ] All prompt files exist and paths are correct
- [ ] All command templates exist
- [ ] All skill directories exist with SKILL.md
- [ ] Permissions are sensible (ask before destructive)
- [ ] Models are valid (`zen/big-pickle` available)

### 8.4 - State File Compatibility

**No changes needed to state files**:
- `state/current.md` - Works as-is
- `state/goals.md` - Works as-is
- `state/todos.md` - Works as-is
- `sessions/` - Append mode works unchanged
- `content/` - Works as-is

**Verification**:
- [ ] Commands can read state files
- [ ] Commands can append to session logs
- [ ] Commands can update current.md
- [ ] Git tracking works for state files

---

## PART 9: RISK MITIGATION

### 9.1 - Potential Issues & Solutions

| Risk | Impact | Mitigation |
|------|--------|-----------|
| OpenCode not reading config | Critical | Test config file syntax early, reference docs |
| Commands don't trigger correctly | High | Verify frontmatter format, test each command |
| Skills not discoverable | Medium | Check SKILL.md filename case, validate YAML |
| Permissions too restrictive | Medium | Test high-risk operations, adjust as needed |
| State files not accessible | High | Verify bash permissions in agents |
| Workflow breaks mid-session | High | Save state early and often in testing |

### 9.2 - Testing Checkpoints

**Checkpoint 1** (After Phase 1):
- [ ] `opencode.json` is valid
- [ ] OpenCode CLI recognizes config
- [ ] Agents are selectable

**Checkpoint 2** (After Phase 2):
- [ ] Each command can be invoked
- [ ] Command prompts execute without error
- [ ] Output appears in TUI

**Checkpoint 3** (After Phase 3):
- [ ] Skills are discoverable in agent tool
- [ ] Each skill can be loaded
- [ ] Skill names are correct

**Checkpoint 4** (After Phase 4):
- [ ] Documentation is accurate
- [ ] Installation guide is complete
- [ ] Architecture is clear

**Checkpoint 5** (After Phase 5):
- [ ] All Claude references removed
- [ ] No broken links in docs
- [ ] Git only shows deliberate changes

**Checkpoint 6** (After Phase 7):
- [ ] Full session workflow succeeds
- [ ] State persists across sessions
- [ ] Agents switch correctly
- [ ] Safety confirmations work

---

## PART 10: QUALITY STANDARDS

### 10.1 - Code Quality Checks

Before final submission:
- [ ] All YAML is valid
- [ ] All JSON is valid
- [ ] All markdown links work
- [ ] No Claude Code references remain
- [ ] All file paths are correct
- [ ] Permissions are sensible
- [ ] No hardcoded secrets

### 10.2 - Documentation Standards

- [ ] README is clear and concise
- [ ] INSTALLATION.md is step-by-step
- [ ] OPENCODE.md covers all features
- [ ] ARCHITECTURE.md is technical but accessible
- [ ] Examples are complete and working
- [ ] Troubleshooting covers common issues

### 10.3 - User Experience Standards

- [ ] First-time setup takes <5 minutes
- [ ] Each command works as expected
- [ ] Error messages are helpful
- [ ] Output is readable in TUI
- [ ] Agent switching is smooth
- [ ] State persistence is reliable

---

## FINAL CHECKLIST

**Before Starting Implementation**:
- [ ] Plan is approved and understood
- [ ] Directory structure is clear
- [ ] File counts are correct (8 commands, 8 skills, 5 prompts)
- [ ] Configuration is complete and tested
- [ ] Documentation outline is comprehensive
- [ ] Risk mitigation is understood

**After Implementation**:
- [ ] All files created in correct locations
- [ ] All Claude Code files deleted
- [ ] Configuration tested and working
- [ ] All commands functional
- [ ] All skills discoverable
- [ ] Documentation complete and accurate
- [ ] Full workflow tested
- [ ] Ready for user adoption

---

## SUMMARY

**What's Happening**:
- MARVIN template is being converted from Claude Code to OpenCode
- All functionality is preserved
- Users who install MARVIN will use OpenCode CLI interface
- Fully open source, no Claude Code dependencies

**What Changes**:
- `.claude/` directory → `.opencode/` directory
- CLAUDE.md system instructions → OPENCODE.md
- Claude Code config → OpenCode config (opencode.json)
- Setup experience → OpenCode installation + MARVIN template

**What Stays the Same**:
- `state/` directory and all state files
- `sessions/` and session logging
- `content/` and content tracking
- Core MARVIN personality and principles
- Command names and functionality
- Skill structure and content

**Ready to Proceed?**

I'm ready to execute all 8 phases systematically. Shall I begin with Phase 1: Configuration Foundation?
