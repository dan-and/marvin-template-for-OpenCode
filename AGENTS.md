# MARVIN Agents Architecture

MARVIN for OpenCode uses a specialized agent system to provide different capabilities for different workflows. This document describes the agents, their roles, and how to use them.

## Overview

MARVIN has **4 specialized agents**:
- **2 Primary Agents**: `marvin` and `planner` (switch with Tab key)
- **2 Subagents**: `executor` and `briefing` (invoked by primary agents or manually via `@`)

Each agent is optimized for specific tasks with different temperature settings, tool access, and safety constraints.

---

## Primary Agents

### 1. MARVIN Agent

**Purpose**: Main interaction agent with full capabilities for all MARVIN workflows

**Type**: Primary (switchable via Tab key)

**Characteristics**:
- **Temperature**: 0.3 (balanced, slightly deterministic)
- **Model**: `zen/big-pickle`
- **Tools**: All enabled (write, edit, bash, webfetch, skill)
- **Permissions**: Ask before high-risk operations

**Capabilities**:
- Session management (`/marvin`, `/end`, `/update`)
- Git operations (`/commit`)
- Report generation (`/report`)
- Template synchronization (`/sync`)
- Content tracking
- Integration management
- General conversation

**When to Use**:
- Default agent for all MARVIN workflows
- Session start and end
- Multi-step processes requiring both analysis and execution
- Integration with external tools

**Safety Constraints**:
```
Bash: Allow all, Ask for (git push, rm)
Edit: Ask for all file modifications
Webfetch: Allow all
```

**Example Usage**:
```
/marvin                    # Start your session
/end                       # End your session
/commit                    # Commit changes
/report                    # Generate weekly report
```

---

### 2. PLANNER Agent

**Purpose**: Analysis and planning without making any changes to your codebase

**Type**: Primary (switchable via Tab key)

**Characteristics**:
- **Temperature**: 0.1 (highly deterministic, focused)
- **Model**: `zen/big-pickle`
- **Tools**: Read-only bash, webfetch, skills (no write/edit)
- **Permissions**: No modifications allowed

**Capabilities**:
- Code analysis and review
- Architecture planning
- Risk assessment
- Research and information gathering
- Workflow planning
- Strategic thinking
- Problem diagnosis

**When to Use**:
- Planning sessions before implementation
- Code review and analysis
- Strategy discussions
- Investigating problems
- Research tasks
- When you want suggestions without changes

**Tool Access**:
```
Bash: Only (grep, find, git log, git diff, ls, pwd)
Write/Edit: Disabled
Webfetch: Enabled
Skills: Enabled
```

**Example Usage**:
```
<TAB>                      # Switch to planner agent
What should our Q1 strategy be?
How would you refactor this module?
What are the risks in this approach?
<TAB>                      # Switch back to marvin when ready to execute
```

**Pro Tip**: Use Tab key to switch to planner when you want MARVIN to analyze something before you commit to changes. Switch back when ready to execute.

---

## Subagents

Subagents are specialized assistants that work within the context of primary agents. They can be invoked manually or automatically by primary agents for specific tasks.

### 3. EXECUTOR Subagent

**Purpose**: Fast, focused implementation of planned tasks

**Type**: Subagent (read-only from UI perspective)

**Characteristics**:
- **Temperature**: 0.3 (balanced, action-oriented)
- **Model**: `zen/big-pickle`
- **Tools**: All enabled (write, edit, bash, webfetch, skill)
- **Permissions**: Allow most operations, ask for (git push)

**Capabilities**:
- Rapid implementation
- Multi-step task execution
- Building features and fixes
- Bulk operations
- Batch changes
- Complex workflows

**When to Use**:
- After planning is complete and you have a clear plan
- Repetitive tasks that need fast execution
- Complex multi-step processes
- When you want execution without extra context loading

**How to Invoke**:
```
@executor Please implement the strategy we just discussed
```

Or the primary MARVIN agent may invoke it automatically for complex tasks:
```
I'll use @executor to handle this multi-step implementation...
```

**Example Workflows**:
1. Use `planner` to create detailed plan
2. Use `@executor` to implement the plan
3. Back to `marvin` for validation and commitment

---

### 4. BRIEFING Subagent

**Purpose**: Generate session briefings and context summaries

**Type**: Subagent (read-only from UI perspective)

**Characteristics**:
- **Temperature**: 0.1 (deterministic, factual)
- **Model**: `zen/big-pickle`
- **Tools**: Read-only bash (find, grep, cat, ls), skills only
- **Permissions**: No external fetches, no modifications

**Capabilities**:
- Session briefing generation
- Context loading and synthesis
- Goal progress tracking
- Priority summarization
- Session history review
- State file analysis

**When to Use**:
- Automatically when `/marvin` command is invoked
- Manual invocation when you need a quick briefing
- Context refreshing mid-session
- Goal progress updates

**How to Invoke**:
```
@briefing Give me a quick morning briefing based on my current state
```

**What It Does**:
1. Reads `state/current.md` for priorities
2. Reads `state/goals.md` for objectives
3. Analyzes today's `sessions/YYYY-MM-DD.md` entries
4. Synthesizes into clear briefing
5. Returns as markdown summary

---

## Agent Switching

### How to Switch Between Primary Agents

**Method 1: Tab Key** (Recommended)
```
Press <TAB> during a session
```
Cycles through available primary agents (marvin → planner → marvin)

**Method 2: Command Entry**
Some OpenCode configurations allow direct agent selection via commands.

### Navigation Between Sessions

When subagents create their own child sessions:

```
<Leader>+Right    # Cycle forward (parent → child1 → child2 → parent)
<Leader>+Left     # Cycle backward (parent ← child1 ← child2 ← parent)
```

This allows seamless navigation between main conversation and specialized work.

---

## Typical Workflows

### Workflow 1: Daily Planning Session

```
1. Start OpenCode
   opencode
   
2. Run /marvin (uses BRIEFING subagent internally)
   /marvin
   → BRIEFING loads your context and goals
   → Returns morning briefing
   
3. Switch to PLANNER for strategic thinking
   <TAB>
   → Now in read-only analysis mode
   → "What should my top 3 priorities be today?"
   → PLANNER analyzes your goals and suggests priorities
   
4. Switch back to MARVIN for execution
   <TAB>
   → Back in full-capability mode
   → Make the decisions, commit them
   
5. End your session
   /end
   → MARVIN summarizes your session
   → Saves to sessions/YYYY-MM-DD.md
   → Updates state/current.md
```

### Workflow 2: Complex Implementation Task

```
1. Start with PLANNER to design
   <TAB>
   → "I need to refactor the authentication system"
   → "Create a detailed implementation plan"
   → PLANNER analyzes codebase and creates plan
   
2. Switch to MARVIN
   <TAB>
   
3. Invoke @executor for implementation
   "Implement the refactoring plan we just created"
   → EXECUTOR handles all the changes
   → Returns summary of modifications
   
4. Back in MARVIN context for review
   "Let me review what executor changed"
   → See all modifications
   → Make any adjustments
   
5. Commit changes
   /commit
```

### Workflow 3: Quick Analysis

```
1. During any session, switch to PLANNER anytime
   <TAB>
   → "Can you analyze this potential issue?"
   → "Is this approach viable?"
   
2. Get analysis without risk of changes
3. Switch back when ready
   <TAB>
```

---

## Agent Configuration

### Where Agents Are Defined

Agents are configured in `.opencode/opencode.json` and use prompts from `.opencode/prompts/`:

```
.opencode/
├── opencode.json           # Agent definitions
├── prompts/
│   ├── marvin.txt         # MARVIN system prompt
│   ├── planner.txt        # PLANNER system prompt
│   ├── executor.txt       # EXECUTOR system prompt
│   └── briefing.txt       # BRIEFING system prompt
```

### Customizing Agents

To customize agent behavior:

1. **Edit prompt files** (`.opencode/prompts/*.txt`):
   - Adjust tone, style, and approach
   - Add specific instructions
   - Refine focus areas

2. **Edit configuration** (`.opencode/opencode.json`):
   - Adjust temperature (0.0=deterministic, 1.0=creative)
   - Change model assignments
   - Modify tool permissions
   - Add new agents

### Creating Custom Agents

To add a new agent:

1. **Create agent config** in `opencode.json`:
```json
{
  "agent": {
    "my-agent": {
      "description": "What this agent does",
      "mode": "subagent",
      "model": "zen/big-pickle",
      "temperature": 0.2,
      "prompt": "{file:./.opencode/prompts/my-agent.txt}",
      "tools": {
        "write": false,
        "bash": true
      }
    }
  }
}
```

2. **Create prompt file** (`.opencode/prompts/my-agent.txt`):
```
Your specialized instructions here...
```

3. **Invoke your agent**:
```
@my-agent Do something specific to this agent
```

---

## Safety & Permissions

### Permission System

Each agent has configured permissions for dangerous operations:

**Levels**:
- `allow` - Operation proceeds without confirmation
- `ask` - User is prompted for approval
- `deny` - Operation is blocked

**Protected Operations**:
- `bash`: All system commands (with specific rules)
- `edit`: File modifications
- `webfetch`: External data fetching

### MARVIN Agent Permissions

```
Bash:
  - Allow: General commands, grep, find
  - Ask: git push, rm operations
  - Deny: git force-push, destructive commands

Edit: Ask for all file modifications
Webfetch: Allow all
```

### PLANNER Agent Permissions

```
Bash: Deny all (read-only only: grep, find, git log)
Edit: Deny all
Webfetch: Allow
```

### Override Permissions

To override permissions for specific operations:

In `.opencode/opencode.json`:
```json
{
  "agent": {
    "marvin": {
      "permission": {
        "bash": {
          "git push": "ask",           # Ask before pushing
          "git force-push": "deny",    # Never allow force push
          "rm *": "ask"                # Ask before any rm
        }
      }
    }
  }
}
```

---

## Temperature Settings Explained

Temperature controls how creative vs. deterministic an agent is:

### Temperature Values

| Value | Behavior | Best For |
|-------|----------|----------|
| 0.0 | Deterministic, focused | PLANNER (consistency) |
| 0.1-0.2 | Mostly focused | BRIEFING (factual summaries) |
| 0.3 | Balanced | MARVIN, EXECUTOR (general work) |
| 0.5 | Creative | Strategy/planning sessions |
| 0.7+ | Very creative | Brainstorming, ideation |

### MARVIN's Temperatures

- **MARVIN**: 0.3 (balanced, reliable, slightly creative)
- **PLANNER**: 0.1 (highly focused, consistent analysis)
- **EXECUTOR**: 0.3 (reliable execution with good judgment)
- **BRIEFING**: 0.1 (factual, consistent summaries)

---

## Troubleshooting Agents

### Agent Not Responding

**Problem**: Agent seems unresponsive or gives wrong type of answer

**Solutions**:
1. Check you're using the right agent (check status bar)
2. For PLANNER issues - don't ask it to make changes (it can't)
3. For EXECUTOR issues - use MARVIN for complex reasoning first
4. Restart session if agent state seems corrupted

### Permissions Blocking Operations

**Problem**: "Permission denied" when trying an operation

**Check**:
1. Verify agent permissions in `opencode.json`
2. Look for `permission` settings on that agent
3. Check `bash`, `edit`, `webfetch` rules
4. May need to ask for confirmation (look for prompt)

### Skills Not Loading

**Problem**: Agent can't load skills

**Check**:
1. Skill file exists in `.opencode/skills/*/SKILL.md`
2. Filename is exactly `SKILL.md` (all caps)
3. YAML frontmatter is valid
4. Skill name matches directory name
5. Check `skill` tool is enabled for that agent

### Model Not Available

**Problem**: Error about `zen/big-pickle` model not found

**Solutions**:
1. Run `opencode models` to see available models
2. Update model in `.opencode/opencode.json`
3. Ensure OpenCode Zen is installed
4. Check internet connection for cloud models

---

## Best Practices

### 1. Use the Right Agent for the Job

- **Planning/Analysis** → Switch to PLANNER
- **General Work** → Use MARVIN (default)
- **Complex Execution** → Invoke @EXECUTOR
- **Session Start** → Let MARVIN call @BRIEFING

### 2. Temperature Awareness

- Lower temperatures (0.1-0.2) for critical operations
- Higher temperatures (0.4-0.6) for creative work
- MARVIN's 0.3 is good for most workflows

### 3. Permission Settings

- Ask for git push (not destructive, but important)
- Ask for rm operations (data loss risk)
- Allow grep/find (safe read operations)
- Deny force-push (can't recover)

### 4. Workflow Optimization

1. Start with PLANNER if uncertain about approach
2. Use MARVIN for decision-making and execution
3. Call @EXECUTOR only for large implementation tasks
4. End sessions with `/end` to save context

### 5. Context Management

- `/marvin` at session start loads all context
- `/update` during session to checkpoint progress
- `/end` to save session summaries
- Use `/report` weekly to synthesize progress

---

## Integration with Skills

Agents can load and use skills for extended capabilities. Skills appear in the agent's tool menu:

```
Available skills:
- marvin: Start MARVIN session with briefing
- daily-briefing: Generate morning briefing
- content-shipped: Track published content
- commit: Git commit workflow
...
```

### Using Skills

**Automatic Loading**:
Agent loads skill when it mentions needing that capability

**Manual Loading** (if agent has skill tool enabled):
Agent can call `skill({ name: "skill-name" })`

### Creating Skills for Agents

Skills are defined in `.opencode/skills/*/SKILL.md`:

```yaml
---
name: my-skill
description: What this skill does
license: MIT
compatibility: opencode
metadata:
  category: workflow
  user-invocable: true
---

## What I Do
- Brief description of capability

## When to Use
- When this skill is useful

## Steps
1. Step 1
2. Step 2
...
```

---

## Advanced: Agent Composition

For complex workflows, you can chain agents:

1. **PLANNER** → Creates detailed plan
2. **MARVIN** → Reviews and adjusts plan
3. **@EXECUTOR** → Executes all changes
4. **MARVIN** → Validates and commits

This provides the benefits of analysis, judgment, execution, and validation all in sequence.

---

## Quick Reference

### Command Cheatsheet

| Action | Command |
|--------|---------|
| Switch agents | Press Tab |
| Start session | `/marvin` |
| End session | `/end` |
| Checkpoint | `/update` |
| Weekly report | `/report` |
| Commit changes | `/commit` |
| Sync template | `/sync` |
| Navigate sessions | `<Leader>+Right/Left` |

### Agent Quick Lookup

| Agent | Type | Temp | Tools | Best For |
|-------|------|------|-------|----------|
| MARVIN | Primary | 0.3 | All | General work |
| PLANNER | Primary | 0.1 | Read-only | Analysis & planning |
| EXECUTOR | Subagent | 0.3 | All | Implementation |
| BRIEFING | Subagent | 0.1 | Read-only | Context loading |

---

## See Also

- `.opencode/opencode.json` - Agent configuration
- `.opencode/prompts/` - Agent system prompts
- `OPENCODE.md` - MARVIN system documentation
- `ARCHITECTURE.md` - Technical architecture
