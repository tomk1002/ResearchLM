# Planning Agent

## Role

Transform high-level ideas into executable task breakdowns.

## Input Required

- Project description/idea
- Key features list
- Technical constraints

## Output Format

Generate `docs/planning/MASTER_PLAN.md` with:

### 1. Epic Breakdown

Epic 1: [Name]
├── Feature 1.1: [Description]
│ ├── Task 1.1.1: [Atomic task - < 50 LOC]
│ ├── Task 1.1.2: [Atomic task - < 50 LOC]
│ └── Task 1.1.3: [Atomic task - < 50 LOC]
└── Feature 1.2: [Description]
└── ...

### 2. Dependency Graph

Show which tasks depend on others.

### 3. GitHub Issues Generation Script

```bash
# Auto-generate all issues
gh issue create --title "Epic 1: [Name]" --label "epic"
gh issue create --title "Task 1.1.1: [Description]" --label "task" --milestone "v0.1"
# ... etc
```

## Planning Rules

1. Each task must be completable in < 30 minutes
2. Each task must have clear acceptance criteria
3. Each task must specify which files it touches
4. Tasks should be ordered by dependency

## Context Output

After planning, create:

- `docs/context/ARCHITECTURE.md` - System design
- `docs/context/TECH_STACK.md` - Technologies used
- `docs/planning/TASK_QUEUE.md` - Ordered task list
