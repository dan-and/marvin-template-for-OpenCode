# Phase 8: Testing - MARVIN OpenCode Migration

This document details the complete testing procedures for verifying MARVIN works correctly in OpenCode.

## Test Overview

Phase 8 consists of 6 major test areas covering all critical workflows:

1. **Test 8.1**: Full Workflow Test (session start to end)
2. **Test 8.2**: Agent Switching Test (Tab key, primary agents)
3. **Test 8.3**: Command Execution Test (all 6 commands)
4. **Test 8.4**: State Persistence Test (across sessions)
5. **Test 8.5**: Subagent Invocation Test (@executor)
6. **Test 8.6**: Safety Confirmation Test (git push)

---

## Test 8.1: Full Workflow Test

**Objective**: Verify complete session workflow from start to finish

**Prerequisites**:
- OpenCode CLI installed
- MARVIN template cloned
- All Phase 1-7 requirements met

**Test Steps**:

1. Start OpenCode with MARVIN template
   ```bash
   opencode
   ```
   
2. Invoke `/marvin` command
   - Expect: Marvin loads context and generates briefing
   - Verify: Briefing contains current priorities and goals
   - Verify: Today's session log is accessible

3. Work on a task (simulated)
   - Example: "Update a state file"
   - Verify: Agent can read and write files

4. Invoke `/update` command
   - Expect: Checkpoint created
   - Verify: Session progress recorded

5. Invoke `/end` command
   - Expect: Session summarized
   - Expect: Session log updated with summary
   - Verify: Session file created in sessions/

6. Exit OpenCode cleanly
   - Expect: No errors
   - Verify: state/ files persist

**Expected Outcome**: ✓ PASS
- Session flows smoothly from start to finish
- All intermediate states save correctly
- No file system errors

**Failure Handling**:
- If step 2 fails: Check .opencode/opencode.json agent assignment
- If step 3 fails: Check file permissions, bash tool access
- If step 5 fails: Check sessions/ directory write permissions

---

## Test 8.2: Agent Switching Test

**Objective**: Verify agent switching mechanism works (primary agents only)

**Prerequisites**:
- OpenCode running with MARVIN template
- Session already started via `/marvin`

**Test Steps**:

1. Verify current agent is MARVIN
   - Expect: Agent indicator shows "marvin"
   - Note: This is for documentation; actual switching happens via Tab key

2. Request analysis that read-only agent can handle
   - Ask: "What's the codebase structure?" (analysis task)
   - Note: In real usage, Tab switches to planner
   
3. Request modification task
   - Ask: "Update state/current.md with new priority"
   - Note: This would require switching back to marvin agent
   - Expect: Agent can write files

**Expected Outcome**: ✓ PASS
- Tab key successfully cycles between primary agents
- Each agent has appropriate tool access
- No mode switching errors

**Failure Handling**:
- If Tab doesn't work: Check OpenCode keybindings in opencode.json
- If wrong agent is active: Verify agent mode settings

---

## Test 8.3: Command Execution Test

**Objective**: Verify all 6 commands execute successfully

**Prerequisites**:
- OpenCode running with MARVIN
- Valid state/ files populated

**Test Procedures**:

### 8.3.1 - /marvin Command
```bash
/marvin
```
- Expect: Briefing generated
- Verify: Output includes:
  - Today's date
  - Top 3 priorities
  - Active goals
  - Suggested focus

### 8.3.2 - /update Command  
```bash
/update
```
- Expect: Checkpoint recorded
- Verify: Session log receives update entry
- Verify: Timestamp captured

### 8.3.3 - /report Command
```bash
/report
```
- Expect: Weekly report generated
- Verify: Report includes:
  - Session count
  - Topics/projects
  - Accomplishments
  - Next week priorities
- Verify: File saved to reports/

### 8.3.4 - /commit Command
```bash
/commit
```
- Expect: Git workflow initiated
- Verify: Shows staged changes
- Verify: Requests commit message
- Verify: Creates commit (may require approval)

### 8.3.5 - /sync Command
```bash
/sync
```
- Expect: Template sync offered
- Verify: Checks for updates
- Verify: Asks for confirmation before syncing
- Note: May indicate no updates available

### 8.3.6 - /end Command
```bash
/end
```
- Expect: Session summarized
- Verify: Asks about accomplishments
- Verify: Session log updated
- Verify: state/ files saved
- Expect: Clean session exit

**Expected Outcome**: ✓ PASS
- All 6 commands execute without errors
- Each produces expected output
- State is updated appropriately

**Failure Handling**:
- If command not found: Check .opencode/opencode.json command definitions
- If frontmatter error: Verify command file YAML syntax
- If agent error: Check agent assignment in command frontmatter

---

## Test 8.4: State Persistence Test

**Objective**: Verify state survives across sessions

**Prerequisites**:
- MARVIN has run at least one session
- state/ files have content

**Test Steps**:

1. Record baseline state
   ```bash
   cat state/current.md
   ```
   - Note current priorities
   - Record last modified time

2. Run session and modify state
   - During `/marvin` or `/update`, state is read
   - Some modifications happen during session

3. Exit session via `/end`
   - Verify state/ files updated
   - Verify git can track changes

4. Start new session
   ```bash
   /marvin
   ```
   - Expect: Briefing reflects previous session context
   - Verify: Previously recorded priorities still present
   - Verify: History is continuous

5. Check git history
   ```bash
   git log --oneline state/
   ```
   - Expect: State changes visible in commits
   - Verify: Timeline is chronological

**Expected Outcome**: ✓ PASS
- State survives between sessions
- Briefing reflects historical context
- Git tracks all changes
- No data loss between sessions

**Failure Handling**:
- If state not saved: Check bash write permissions
- If history gaps: Check git commits
- If stale briefing: Check state file freshness

---

## Test 8.5: Subagent Invocation Test

**Objective**: Verify @executor and @briefing subagents work correctly

**Prerequisites**:
- MARVIN session running
- Complex task available

**Test Steps**:

1. Request large implementation task
   - Example: "Implement feature X"
   - Observe: Marvin may invoke @executor internally

2. Request briefing during session
   - Marvin can invoke @briefing for context refresh
   - Expect: Briefing generates fresh summary

3. Manually invoke subagent (if supported)
   - Try: "@executor implement this change"
   - Expect: Executor focuses on implementation
   - Verify: No prompt overhead

**Expected Outcome**: ✓ PASS
- Subagents activate when needed
- Executor is faster/more focused
- Briefing generates accurate summaries
- Smooth handoffs between agents

**Failure Handling**:
- If subagent not found: Check executor/briefing in opencode.json
- If slow invocation: Check agent startup
- If missing context: Verify state loading in subagents

---

## Test 8.6: Safety Confirmation Test

**Objective**: Verify permission system protects against destructive operations

**Prerequisites**:
- MARVIN session running
- Git repository with uncommitted changes

**Test Steps**:

1. Set up test scenario
   - Modify a file intentionally
   - Stage change: `git add state/current.md`

2. Request git push via `/commit`
   ```bash
   /commit
   ```
   - Expect: Marvin shows staged changes
   - Expect: Asks for commit message

3. Verify push confirmation
   - If agent tries: `git push origin main`
   - Expect: Safety prompt appears
   - Expect: Requires explicit approval
   - Note: Per config, git push is set to "ask"

4. Request rm operation (simulated)
   - If during session agent suggests file deletion
   - Expect: Safety confirmation required
   - Expect: Cannot proceed without approval
   - Note: Per config, rm operations require approval

5. Verify git force-push is blocked
   - If agent ever attempts: `git push --force`
   - Expect: Operation completely denied
   - Note: Per config, git force-push is set to "deny"

**Expected Outcome**: ✓ PASS
- Git push requires confirmation
- Destructive rm requires confirmation
- Force-push is completely blocked
- Safety system prevents accidental damage

**Failure Handling**:
- If no confirmation appears: Check permission config in opencode.json
- If blocked incorrectly: Verify permission rules for git push
- If force-push allowed: Check git force-push is set to deny

---

## Test Coverage Matrix

| Test | Area | Status | Notes |
|------|------|--------|-------|
| 8.1 | Full Workflow | TBD | Complete session cycle |
| 8.2 | Agent Switching | TBD | Primary agent switching |
| 8.3.1 | /marvin Command | TBD | Session start |
| 8.3.2 | /update Command | TBD | Checkpoint |
| 8.3.3 | /report Command | TBD | Weekly report |
| 8.3.4 | /commit Command | TBD | Git workflow |
| 8.3.5 | /sync Command | TBD | Template sync |
| 8.3.6 | /end Command | TBD | Session end |
| 8.4 | State Persistence | TBD | Cross-session state |
| 8.5 | Subagent Invocation | TBD | @executor and @briefing |
| 8.6 | Safety Confirmations | TBD | Permission system |

---

## Quick Test Checklist

For rapid verification:

- [ ] `opencode.json` is valid JSON
- [ ] All 5 prompt files exist
- [ ] All 6 command files exist
- [ ] All 8 skill SKILL.md files exist
- [ ] `/marvin` command executes
- [ ] `/end` command executes
- [ ] Session log created
- [ ] State files persist
- [ ] Git commits succeed
- [ ] No errors in TUI output

---

## Success Criteria

Phase 8 is complete when:

✓ All 6 test areas execute without critical errors  
✓ State persists across sessions  
✓ All 6 commands work as designed  
✓ Agent switching is smooth  
✓ Safety confirmations work  
✓ Subagents respond appropriately  

---

## Known Limitations

- Agent switching (Tab key) is OpenCode feature - may vary by install
- MCP server integration requires user configuration
- Some tests require actual OpenCode CLI environment
- Subagent testing requires complex task triggering

---

## Next Steps

After Phase 8 Testing completes:

1. Document any issues found
2. Create TROUBLESHOOTING.md for common problems
3. Add FAQ section to OPENCODE.md
4. Create quick reference guide
5. Final review of all files

See MIGRATION_PLAN.md Phase 9+ for advanced features.
