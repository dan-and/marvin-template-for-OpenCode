---
description: Sync with MARVIN template updates
agent: marvin
---

# Sync with MARVIN Template Updates

Check for and get updates from the MARVIN template repository, applying only safe, non-destructive changes to user's installation.

## Instructions

### 1. Check Repository Status
```bash
git remote -v
git status
```

### 2. Check for Updates
Compare local repository with remote to see what updates are available:
```bash
git fetch
git log HEAD..origin/main --oneline
```

### 3. Identify Safe Updates
Only apply changes that are:
- Documentation updates (README.md, AGENTS.md, etc.)
- Configuration improvements (non-destructive)
- New commands or skills
- Bug fixes

**NEVER APPLY CHANGES TO**:
- `state/` directory files (user's personal data)
- `sessions/` directory files (user's session history)
- `content/` directory files (user's shipped content)

### 4. Ask for Permission
Before syncing any changes, present to the user:
- What updates are available
- Which files will be modified
- Confirm: "I found [X] updates. Shall I apply them?"

Wait for explicit user approval before proceeding with any git operations.

## Update Categories

### Documentation Updates
If new documentation files are available:
- Explain what's new
- Show differences if requested

### Configuration Updates
If `.opencode/opencode.json` has updates:
- Explain what's changed
- Backup current config before applying
- Apply new configuration

### Command Updates
If new commands are added:
- List new commands
- Explain their purpose
- Add to `.opencode/commands/` directory

### Skill Updates
If new skills are added:
- List new skills
- Explain their capabilities
- Add to `.opencode/skills/` directory

## Sync Process

### Step 1: Fetch
```bash
git fetch origin
```

### Step 2: Check Updates
```bash
git log HEAD..origin/main --oneline | head -20
```

### Step 3: Present to User
Summarize updates available:
- Documentation: [count] new files
- Configuration: [changes] new features
- Commands: [count] new commands
- Skills: [count] new skills

### Step 4: Get Approval
Present update summary and ask:
"Do you want to apply these [X] updates? I will only modify safe files and preserve all your personal data in state/, sessions/, and content/."

### Step 5: Apply Updates
IF APPROVED:
```bash
git pull origin main
```

Then verify:
```bash
git status
```

## Notes

- MARVIN template GitHub URL may change in the future
- Always ask for user permission before syncing
- Never overwrite user's state files
- Always preserve user's session logs
- Always preserve user's content tracking
- This command is for safe, non-destructive updates only

## Important

The MARVIN template repository is the reference implementation. Users configure their own MARVIN system based on this template. Syncing should only add new features and improvements, never change or remove user's configuration or data.

If the GitHub URL changes or the project structure changes significantly, document this clearly to users.
