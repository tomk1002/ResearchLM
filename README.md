# AI-Driven Development Starter Kit v2

An improved workflow for AI-orchestrated development with Claude Code, featuring:

- **Adaptive task sizing** (not fixed 50 LOC)
- **Human intervention protocol** for database, API keys, MCP setup
- **Centralized context rules** (no duplication)
- **Clear blocker management**

## Quick Setup

```bash
# 1. Create your project
mkdir my-project && cd my-project
git init

# 2. Copy starter kit files
cp -r path/to/starter-kit-v2/* .
cp -r path/to/starter-kit-v2/.* . 2>/dev/null

# 3. Install GitHub CLI (if not installed)
# macOS: brew install gh
# Ubuntu: sudo apt install gh

# 4. Authenticate
gh auth login
claude auth login

# 5. Customize CLAUDE.md with your project details

# 6. Commit the structure
git add -A
git commit -m "chore: initialize AI-driven development workflow"

# 7. Start Claude Code
claude
```

## Directory Structure

```
.
├── CLAUDE.md                           # Main config (customize this!)
├── .claude/
│   ├── agents/
│   │   ├── 00-orchestrator.md         # Session coordination
│   │   ├── 01-planner.md              # Project planning
│   │   ├── 02-implementer.md          # Code implementation
│   │   └── 03-reviewer.md             # Code review
│   └── commands/
│       ├── start-task.md              # /project:start-task
│       ├── complete-task.md           # /project:complete-task
│       ├── prune-context.md           # /project:prune-context
│       ├── needs-human.md             # /project:needs-human ← NEW
│       └── resume-task.md             # /project:resume-task ← NEW
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── task.md                    # Development task template
│   │   └── setup.md                   # Human setup task ← NEW
│   └── pull_request_template.md
└── docs/
    ├── context/
    │   ├── _CONTEXT_RULES.md          # Context management rules ← NEW
    │   ├── CURRENT_TASK.md            # Current task only
    │   ├── BLOCKERS.md                # Human intervention needed ← NEW
    │   ├── ARCHITECTURE.md            # System design
    │   └── DECISIONS.md               # Decision log
    ├── planning/
    │   └── TASK_QUEUE.md
    └── archives/
```

## Key Improvements in v2

### 1. Adaptive Task Sizing

| Size   | LOC     | Use When                                  |
| ------ | ------- | ----------------------------------------- |
| Tiny   | < 50    | Complex logic, unfamiliar code, high-risk |
| Small  | 50-150  | Standard features, clear patterns         |
| Medium | 150-300 | Self-contained, boilerplate-heavy         |
| Large  | 300+    | Scaffolding, migrations, mechanical       |

**Choose based on complexity, not a fixed limit.**

### 2. Human Intervention Protocol

When Claude encounters something it can't do alone:

```
/project:needs-human [description]
```

This will:

1. Stop current work safely
2. Document the blocker in `BLOCKERS.md`
3. Update the GitHub issue
4. Suggest alternative unblocked tasks
5. Provide clear instructions for the human

After human resolves it:

```
/project:resume-task [issue-number]
```

### 3. Centralized Context Rules

All context management rules live in ONE file:

```
docs/context/_CONTEXT_RULES.md
```

Agents reference this file instead of duplicating rules.

### 4. Clear Blocker Tracking

Active blockers are tracked in:

```
docs/context/BLOCKERS.md
```

The orchestrator checks this at session start.

## First Session

```bash
claude

# 1. Tell Claude your idea
"I want to build [description]. Help me plan it."

# 2. Claude will read the planner agent and create:
# - GitHub issues (including setup tasks labeled 'needs-human')
# - MASTER_PLAN.md
# - TASK_QUEUE.md
# - ARCHITECTURE.md

# 3. Complete any setup tasks (database, API keys, etc.)

# 4. Start development
/project:start-task [first-non-blocked-issue]
```

## Daily Workflow

```bash
claude

# Check status
"What's the current status?"

# Work on next task
/project:start-task [issue-number]

# ... do the work ...

# Complete task
/project:complete-task

# If you hit a blocker
/project:needs-human [what's needed]

# After human resolves blocker
/project:resume-task [issue-number]

# End of session: clean up context
/project:prune-context
```

## Customization Checklist

- [ ] Update project name in `CLAUDE.md`
- [ ] Add your bash commands
- [ ] Configure tech stack in `ARCHITECTURE.md`
- [ ] Set up MCP servers table in `CLAUDE.md`
- [ ] Adjust task size preferences if needed
- [ ] Add project-specific quality gates

## Context Budget

| File                | Max Lines | Purpose              |
| ------------------- | --------- | -------------------- |
| `_CONTEXT_RULES.md` | ~200      | Rules (don't modify) |
| `CURRENT_TASK.md`   | 100       | Active task only     |
| `BLOCKERS.md`       | 50        | Active blockers only |
| `ARCHITECTURE.md`   | 200       | System design        |
| `DECISIONS.md`      | 300       | Decision log         |
| **Total Active**    | **~650**  | Keep under 800       |

## Commands Reference

| Command                        | Purpose                         |
| ------------------------------ | ------------------------------- |
| `/project:start-task [issue]`  | Begin a task                    |
| `/project:complete-task`       | Finish task, create PR          |
| `/project:needs-human [desc]`  | Request human help              |
| `/project:resume-task [issue]` | Continue after blocker resolved |
| `/project:prune-context`       | Clean up context files          |
| `/clear`                       | Reset Claude's context window   |
