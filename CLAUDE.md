# Project: [researchLM]

## Prime Directive

> Optimize context usage by prioritizing relevance, summarizing aggressively, isolating agent memory, and pruning stale or low-signal information to prevent context rot.

## Project Overview

[Brief 2-3 sentence description of what this project does]

---

## Context Management

**Read `docs/context/_CONTEXT_RULES.md` for all context management rules.**

- Keep total active context < 800 lines
- Prune after each task completion
- Archive aggressively

---

## Task Sizing (Adaptive)

Choose size based on task characteristics, NOT a fixed limit:

| Size       | Lines   | When to Use                                                           |
| ---------- | ------- | --------------------------------------------------------------------- |
| **Tiny**   | < 50    | Complex logic, unfamiliar code, many edge cases, learning phase       |
| **Small**  | 50-150  | Standard features, well-understood patterns, good test coverage       |
| **Medium** | 150-300 | Self-contained features, boilerplate-heavy, mechanical changes        |
| **Large**  | 300+    | Scaffolding, migrations, refactors (use autonomous mode in container) |

**Default to smaller when uncertain.** Increase size as you gain confidence with the codebase.

---

## Human Intervention Protocol

**Claude MUST stop and request human help for:**

- Database connections / credentials
- API keys / secrets
- OAuth / authentication setup
- MCP server configuration
- External service registration
- Production deployments
- Any access Claude doesn't have

**When blocked:** Update `docs/context/BLOCKERS.md` and notify human.

See `/project:needs-human` command for proper protocol.

---

## MCP Servers

| Server       | Purpose            | Status          |
| ------------ | ------------------ | --------------- |
| `filesystem` | File operations    | ✅ Built-in     |
| `git`        | Version control    | ✅ Built-in     |
| `github`     | Issues, PRs        | ⬜ Setup needed |
| `postgres`   | Database access    | ⬜ Setup needed |
| `puppeteer`  | Browser automation | ⬜ Setup needed |
| [add more]   | [purpose]          | ⬜ Setup needed |

**To configure MCP:**

```bash
# Add a server
claude mcp add [server-name]

# Or add to .mcp.json for team sharing
```

---

## Bash Commands

```bash
# Development
npm run dev          # Start dev server
npm test             # Run tests
npm run lint         # Lint code
npm run typecheck    # Type check (if TS)
npm run build        # Production build

# Git workflow
git checkout -b feat/[issue]-[name]   # New feature branch
git add -A && git commit -m "..."     # Commit
git push -u origin HEAD               # Push

# GitHub CLI
gh issue list                         # List issues
gh issue view [number]                # View issue
gh issue create --title "..."         # Create issue
gh pr create --fill                   # Create PR
gh pr merge --squash --delete-branch  # Merge PR
```

---

## Git Workflow

- **Branch naming**: `feat/[issue-number]-brief-description`
- **Commit format**: `[type](scope): description (#issue)`
- **Types**: feat, fix, docs, style, refactor, test, chore
- **Rule**: Never push directly to main - always use PRs

---

## Active Context Files

| File                             | Purpose                  | Read When                   |
| -------------------------------- | ------------------------ | --------------------------- |
| `docs/context/_CONTEXT_RULES.md` | Context management rules | Always                      |
| `docs/context/CURRENT_TASK.md`   | Current task details     | Always                      |
| `docs/context/BLOCKERS.md`       | Human help needed        | If exists                   |
| `docs/context/ARCHITECTURE.md`   | System design            | For structural work         |
| `docs/context/DECISIONS.md`      | Decision history         | For context on past choices |

---

## Agent Files (in `.claude/agents/`)

| Agent                | Purpose            | Use For                |
| -------------------- | ------------------ | ---------------------- |
| `00-orchestrator.md` | Task coordination  | Start of each session  |
| `01-planner.md`      | Breaking down work | New features, planning |
| `02-implementer.md`  | Writing code       | Implementation tasks   |
| `03-reviewer.md`     | Code review        | PR reviews             |

---

## Quality Gates

Before ANY commit:

- [ ] Tests pass (`npm test`)
- [ ] Linter passes (`npm run lint`)
- [ ] Type check passes (`npm run typecheck`)
- [ ] Context files updated
- [ ] No blockers for this task

---

## Tech Stack

- **Frontend**: [framework]
- **Backend**: [framework]
- **Database**: [type]
- **Hosting**: [platform]
- **Key libraries**: [list]

---

## File Conventions

- Components: `PascalCase.tsx`
- Utilities: `camelCase.ts`
- Tests: `*.test.ts` or `*.spec.ts`
- Types: `types.ts` or inline

---

## Important Reminders

1. Read `_CONTEXT_RULES.md` for context management
2. Check `BLOCKERS.md` before starting work
3. Use adaptive task sizing (not fixed 50 LOC)
4. Stop and ask for human help when needed
5. Prune context after each task
6. When stuck: `/clear` and re-read agent file
