# AI-Driven Development Workflow: Complete Execution Guide (v2)

## What's New in v2

1. **Adaptive Task Sizing** - Choose size based on complexity, not a fixed 50 LOC limit
2. **Human Intervention Protocol** - Proper handling of database, API keys, MCP, OAuth
3. **Centralized Context Rules** - Single source of truth in `_CONTEXT_RULES.md`
4. **Blocker Management** - Clear tracking and resolution workflow

---

## Phase 1: Environment Setup (15-30 minutes)

### Step 1.1: Install Prerequisites

```bash
# 1. Install Claude Code globally
npm install -g @anthropic-ai/claude-code

# 2. Install GitHub CLI
# macOS:
brew install gh
# Ubuntu/Debian:
sudo apt install gh
# Windows:
winget install --id GitHub.cli

# 3. Authenticate both tools
claude auth login
gh auth login

# 4. Verify installations
claude --version
gh --version
```

### Step 1.2: Create Project Structure

```bash
mkdir your-project && cd your-project
git init

# Create the full directory structure
mkdir -p .claude/commands
mkdir -p .claude/agents
mkdir -p .github/ISSUE_TEMPLATE
mkdir -p docs/context
mkdir -p docs/planning
mkdir -p docs/archives
```

### Step 1.3: Copy Starter Kit

Download and extract `ai-dev-starter-kit-v2.zip`, then:

```bash
cp -r starter-kit-v2/* your-project/
cp -r starter-kit-v2/.* your-project/ 2>/dev/null
```

---

## Phase 2: Understanding the System

### Core Files

| File                             | Purpose                         | When to Read         |
| -------------------------------- | ------------------------------- | -------------------- |
| `CLAUDE.md`                      | Project config, commands, rules | Always loaded        |
| `docs/context/_CONTEXT_RULES.md` | Context management rules        | Referenced by agents |
| `docs/context/CURRENT_TASK.md`   | Active task details             | Each task            |
| `docs/context/BLOCKERS.md`       | Human intervention needed       | When blocked         |

### Agent Files

| Agent                | Purpose                | Use For               |
| -------------------- | ---------------------- | --------------------- |
| `00-orchestrator.md` | Session coordination   | Start of each session |
| `01-planner.md`      | Breaking down projects | Planning phase        |
| `02-implementer.md`  | Writing code           | Development tasks     |
| `03-reviewer.md`     | Code review            | PR reviews            |

### Slash Commands

| Command                        | Purpose                         |
| ------------------------------ | ------------------------------- |
| `/project:start-task [issue]`  | Begin working on a task         |
| `/project:complete-task`       | Finish task, create PR          |
| `/project:needs-human [desc]`  | Stop and request human help     |
| `/project:resume-task [issue]` | Continue after blocker resolved |
| `/project:prune-context`       | Clean up context files          |

---

## Phase 3: Task Sizing (Adaptive)

**Don't default to tiny tasks.** Choose based on task characteristics:

### Tiny Tasks (< 50 LOC)

**When to use:**

- Complex logic with many edge cases
- Unfamiliar codebase or patterns
- High-risk changes (auth, payments, data migrations)
- Learning/exploring phase

**Protocol:** Full 4-phase (Explore → Plan → Implement → Verify)

### Small Tasks (50-150 LOC)

**When to use:**

- Standard CRUD operations
- Well-understood patterns exist
- Good test coverage to catch issues
- Single component or API endpoint

**Protocol:** Condensed (Explore+Plan combined → Implement → Verify)

### Medium Tasks (150-300 LOC)

**When to use:**

- Self-contained features
- Boilerplate-heavy work
- Clear patterns to follow
- Mechanical refactoring

**Protocol:** Efficient (Quick explore → Implement in chunks → Verify)

### Large Tasks (300+ LOC)

**When to use:**

- Scaffolding and code generation
- Database migrations
- File reorganization
- Autonomous container mode

**Protocol:** Batch (Implement in batches → Checkpoint commits → Final verify)

---

## Phase 4: Human Intervention Protocol

### What Requires Human Intervention

| Category              | Examples                             |
| --------------------- | ------------------------------------ |
| **Credentials**       | Database URLs, API keys, secrets     |
| **Auth Setup**        | OAuth apps, SSO config, JWT secrets  |
| **External Services** | Stripe, AWS, third-party APIs        |
| **MCP Servers**       | Unconfigured servers needed for task |
| **Infrastructure**    | Hosting, DNS, CI/CD                  |
| **Permissions**       | Admin access, deployment rights      |

### When Claude Hits a Blocker

Claude runs `/project:needs-human`:

1. **Stops immediately** (no workarounds)
2. **Commits WIP** to preserve progress
3. **Documents in BLOCKERS.md** with clear instructions
4. **Updates GitHub issue** with blocker reference
5. **Suggests alternative tasks** without this blocker
6. **Notifies human** with specific instructions

### Human Resolution Flow

```
┌─────────────────────────────────────────────────────────────┐
│  Claude working on Task #123                                │
│                                                             │
│  Encounters: "DATABASE_URL not configured"                  │
│                                                             │
│  → /project:needs-human                                     │
│     • Stops work                                            │
│     • Updates BLOCKERS.md                                   │
│     • Comments on GitHub issue                              │
│     • Shows human exactly what to do                        │
│                                                             │
│  → Claude works on Task #124 (not blocked) OR waits        │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  Human receives clear instructions:                         │
│                                                             │
│  "Set up PostgreSQL and add DATABASE_URL to .env"          │
│  1. Create database: createdb myapp                         │
│  2. Add to .env: DATABASE_URL=postgresql://...              │
│  3. Tell Claude: "Database is ready"                        │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  Human says: "Database is ready" or runs:                   │
│  /project:resume-task 123                                   │
│                                                             │
│  → Claude verifies blocker is resolved                      │
│  → Clears entry from BLOCKERS.md                            │
│  → Continues with Task #123                                 │
└─────────────────────────────────────────────────────────────┘
```

---

## Phase 5: Context Management

### The Prime Directive

> **Optimize context usage by prioritizing relevance, summarizing aggressively, isolating agent memory, and pruning stale or low-signal information to prevent context rot.**

This appears in every agent file as a constant reminder.

### Context Budget

| File                | Max Lines | Update Frequency    |
| ------------------- | --------- | ------------------- |
| `_CONTEXT_RULES.md` | ~200      | Never (reference)   |
| `CURRENT_TASK.md`   | 100       | Replace each task   |
| `BLOCKERS.md`       | 50        | Clear when resolved |
| `ARCHITECTURE.md`   | 200       | When design changes |
| `DECISIONS.md`      | 300       | Append only         |
| **Total Target**    | **~650**  | Keep under 800      |

### Context Rotation Rules

| File              | Rotation Rule                       |
| ----------------- | ----------------------------------- |
| `CURRENT_TASK.md` | **Replace entirely** each task      |
| `BLOCKERS.md`     | **Clear immediately** when resolved |
| `ARCHITECTURE.md` | **Update only** when design changes |
| `DECISIONS.md`    | **Append only**, summarize monthly  |

### When to Prune

| Trigger               | Action                |
| --------------------- | --------------------- |
| After each task       | Quick prune           |
| Total > 600 lines     | Full prune            |
| Total > 800 lines     | Emergency prune       |
| Claude seems confused | Full prune + `/clear` |
| Start of session      | Health check          |

---

## Phase 6: Execution Workflow

### Initial Planning Session

```bash
cd your-project
claude

# Tell Claude your idea
"I want to build [detailed description of your software service].
Please read the planner agent and create a complete breakdown."
```

Claude will:

1. Ask clarifying questions
2. Identify human dependencies (database, APIs, etc.)
3. Create GitHub issues (including `needs-human` setup tasks)
4. Generate `MASTER_PLAN.md` and `TASK_QUEUE.md`
5. Update `ARCHITECTURE.md`

### Setup Phase (Human)

Before development can start, complete setup tasks:

```bash
# See what setup is needed
gh issue list --label needs-human

# Complete each setup task
# (database, API keys, OAuth, etc.)

# Mark as done
gh issue comment [number] --body "Setup complete"
```

### Daily Development Loop

```bash
claude

# 1. Check status
"What's the current state?"
# Claude reads BLOCKERS.md, CURRENT_TASK.md, checks gh issues

# 2. Start a task
/project:start-task [issue-number]

# 3. Work through the task
# Claude follows appropriate protocol based on task size

# 4. If blocked, Claude will automatically:
/project:needs-human [description]

# 5. Complete the task
/project:complete-task

# 6. End of session
/project:prune-context
```

### Handling Blockers Mid-Session

```
Claude: "I need DATABASE_URL to continue. Running /project:needs-human..."

🛑 HUMAN INTERVENTION REQUIRED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
WHAT I NEED: PostgreSQL database connection
INSTRUCTIONS:
1. Create database
2. Add DATABASE_URL to .env
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
MEANWHILE: I can work on #45 (no database needed)
Want me to start that?

You: "Yes, work on #45"
# OR
You: [set up database] "Database is ready, continue with #42"

Claude: "Verifying... ✅ Connection works. Resuming #42."
```

---

## Phase 7: Multi-Agent Patterns

### Pattern 1: Parallel Git Worktrees

```bash
# Create worktrees for independent features
git worktree add ../project-feature-a feat/feature-a
git worktree add ../project-feature-b feat/feature-b

# Run Claude in each (separate terminal tabs)
cd ../project-feature-a && claude
cd ../project-feature-b && claude
```

### Pattern 2: Writer + Reviewer

```bash
# Terminal 1: Write code
claude
/project:start-task 123
# ... complete implementation ...
/project:complete-task

# Terminal 2: Fresh context for review
claude
"Review PR #[number] using the reviewer agent protocol"
```

### Pattern 3: Autonomous Mode (Container)

For low-risk, mechanical tasks:

```bash
docker run -it --rm \
  -v $(pwd):/workspace \
  --network none \
  your-dev-container \
  claude --dangerously-skip-permissions \
  -p "Complete tasks #45, #46, #47 following all protocols.
      Stop if any task fails tests or hits a blocker."
```

---

## Quick Reference

### Essential Commands

| Command                    | What It Does                  |
| -------------------------- | ----------------------------- |
| `/project:start-task [#]`  | Begin a task                  |
| `/project:complete-task`   | Finish and PR                 |
| `/project:needs-human`     | Request human help            |
| `/project:resume-task [#]` | Continue after blocker        |
| `/project:prune-context`   | Clean up context              |
| `/clear`                   | Reset Claude's context window |

### GitHub CLI Essentials

```bash
# Issues
gh issue list                          # List open issues
gh issue view [number]                 # View details
gh issue create --title "..."          # Create issue
gh issue comment [number] --body "..." # Add comment

# Pull Requests
gh pr create --fill                    # Create PR
gh pr view [number]                    # View PR
gh pr merge --squash --delete-branch   # Merge PR

# Labels for filtering
gh issue list --label "needs-human"    # Setup tasks
gh issue list --label "task"           # Dev tasks
```

### Task Size Quick Reference

| Complexity               | Size   | LOC     | Protocol     |
| ------------------------ | ------ | ------- | ------------ |
| High risk, unfamiliar    | Tiny   | < 50    | Full 4-phase |
| Standard, clear patterns | Small  | 50-150  | Condensed    |
| Self-contained feature   | Medium | 150-300 | Efficient    |
| Scaffolding, mechanical  | Large  | 300+    | Batch        |

### Context Health

```bash
# Check context size
wc -l docs/context/*.md

# Target: < 650 lines
# Warning: > 800 lines
# Action: /project:prune-context
```

---

## Troubleshooting

### Claude Seems Confused or Inconsistent

1. Check context size: `wc -l docs/context/*.md`
2. Run `/project:prune-context`
3. Use `/clear` to reset context window
4. Re-read only essential files

### Task Taking Too Long

1. Check if task size is appropriate
2. Consider breaking into smaller tasks
3. Look for undocumented blockers
4. Verify dependencies are actually complete

### Blocker Not Resolving

1. Check `BLOCKERS.md` for exact requirements
2. Verify human completed ALL steps
3. Run `/project:resume-task` to re-verify
4. Check for additional blockers revealed

### Lost Track of Progress

```bash
# GitHub is the source of truth
gh issue list --state all
gh pr list --state all
git log --oneline -20

# Rebuild context from GitHub
gh issue view [current-issue]
```

### Context Files Corrupted

```bash
# Emergency reset
mkdir -p docs/archives/$(date +%Y-%m-%d)
mv docs/context/CURRENT_TASK.md docs/archives/$(date +%Y-%m-%d)/
mv docs/context/ARCHITECTURE.md docs/archives/$(date +%Y-%m-%d)/

# Rebuild from scratch
gh issue view [current] > docs/context/CURRENT_TASK.md
# Edit to proper format

# Keep DECISIONS.md (append-only log)
# Keep BLOCKERS.md if has active blockers
```

---

## Customization Tips

### For Solo Developers

- Enable self-merge for low-risk PRs
- Use larger task sizes once comfortable
- Reduce ceremony in PR templates
- Consider autonomous mode for boilerplate

### For Teams

- Keep all setup tasks as `needs-human` issues
- Require PR reviews (no self-merge)
- Use smaller task sizes for consistency
- Check in `.claude/` directory for shared config

### For Complex Projects

- Add more specialized agents (e.g., `04-debugger.md`, `05-migrator.md`)
- Create domain-specific slash commands
- Expand `ARCHITECTURE.md` with subsystem details
- Consider multiple `CLAUDE.md` files for monorepos

### For Learning

- Use Tiny tasks exclusively at first
- Keep `DECISIONS.md` detailed for reference
- Don't prune aggressively (keep more context)
- Review every PR manually before merge

---

## Files Included in Starter Kit v2

```
starter-kit-v2/
├── CLAUDE.md                              # Main config
├── README.md                              # Quick start guide
├── .claude/
│   ├── agents/
│   │   ├── 00-orchestrator.md            # Session coordination
│   │   ├── 01-planner.md                 # Project planning
│   │   ├── 02-implementer.md             # Implementation
│   │   └── 03-reviewer.md                # Code review
│   └── commands/
│       ├── start-task.md                 # Begin task
│       ├── complete-task.md              # Finish task
│       ├── needs-human.md                # Request help
│       ├── resume-task.md                # Continue after blocker
│       └── prune-context.md              # Clean context
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── task.md                       # Dev task template
│   │   └── setup.md                      # Human setup template
│   └── pull_request_template.md          # PR template
└── docs/
    ├── context/
    │   ├── _CONTEXT_RULES.md             # Context management rules
    │   ├── CURRENT_TASK.md               # Current task
    │   ├── BLOCKERS.md                   # Active blockers
    │   ├── ARCHITECTURE.md               # System design
    │   └── DECISIONS.md                  # Decision log
    ├── planning/
    │   └── TASK_QUEUE.md                 # Task queue
    └── archives/
        └── README.md                     # Archive instructions
```

---

## Next Steps

1. **Download the starter kit** (`ai-dev-starter-kit-v2.zip`)
2. **Extract to your project directory**
3. **Customize `CLAUDE.md`** with your project details
4. **Start Claude Code** and describe your idea
5. **Complete setup tasks** flagged as `needs-human`
6. **Begin development** with `/project:start-task`
7. **Iterate** on the workflow based on what works for you

---

## Changelog

### v2 (Current)

- Added adaptive task sizing (Tiny/Small/Medium/Large)
- Added human intervention protocol (`/project:needs-human`, `/project:resume-task`)
- Added `BLOCKERS.md` for tracking human dependencies
- Centralized context rules in `_CONTEXT_RULES.md`
- Added setup issue template for human tasks
- Removed duplicate context rules from agent files
- Updated all agents to reference central rules

### v1

- Initial release with fixed 50 LOC task size
- Basic context management
- Four agents (orchestrator, planner, implementer, reviewer)
- Three commands (start-task, complete-task, prune-context)
