---
description: Review and commit git changes
agent: marvin
template: "{file:./.opencode/commands/commit.md}"
---

# /commit - Git Workflow

Review uncommitted changes and create logical, well-organized commits for the MARVIN migration to OpenCode.

## Instructions

### 1. Check Current State
Run `git status` and `git diff --stat` to see all uncommitted changes.

### 2. Group Changes
Identify logical groupings from changes:

| Group | Files | Commit Type |
|-------|-------|-------------|
| Features/Scripts | `src/*.py`, `*.js` | `feat:` |
| Config | `CLAUDE.md`, `*.json` | `chore:` |
| Content | `content/`, `research/` | `content:` |
| State/Sessions | `state/`, `sessions/` | `chore:` |
| Docs | `*.md` (non-state) | `docs:` |

### 3. Create Commits
For each logical group, create a focused commit:

```bash
git add <relevant-files>
git commit -m "$(cat <<'EOF'
<type>: <short description>

<detail>: <longer description>
EOF
)"
```

### 4. Commit Order
1. Dependencies first (if B uses A, commit A first)
2. Features before docs
3. Content before state
4. **State/sessions always last**

### 5. Push (if requested)
After all commits are created:
```bash
git push
```

## Commit Message Types

| Type | Use For |
|-------|---------|
| `feat:` | New features, scripts, or integrations |
| `fix:` | Bug fixes or error corrections |
| `docs:` | Documentation, setup guides, or README changes |
| `config:` | Configuration file changes |
| `chore:` | Maintenance tasks, refactoring, or repository management |
| `content:` | Blog posts, research, content files |

## Note for OpenCode Migration

The MARVIN migration has several commits that should be grouped:

1. **Configuration Files** (`.opencode/opencode.json`, `.opencode/prompts/*.txt`, `.opencode/commands/*.md`)
   - Commit Type: `config:` or `docs:`
   - Group these together for migration documentation

2. **Planning Documentation** (`MIGRATION_PLAN.md`, `AGENTS.md`, `REFERENCE_ANALYSIS.md`, etc.)
   - Commit Type: `docs:`
   - Group all planning docs in one commit

3. **Analysis Documentation** (`REFERENCE_ANALYSIS.md`, `ANALYSIS_COMPLETE.md`)
   - Commit Type: `docs:`
   - Group all analysis docs together

Make sure to commit state and session files LAST to avoid conflicts.

## Notes

- Keep commits atomic and focused on single logical changes
- Use present tense in commit messages (e.g., "Adds configuration" not "Adding configuration")
- Be descriptive in commit body but concise in subject line
- NEVER merge unrelated changes in one commit
- Reference related issues or documentation if applicable
