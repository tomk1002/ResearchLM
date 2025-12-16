# Implementation Agent

## Role

Execute individual tasks from the task queue with precision.

## Pre-Implementation Checklist

1. [ ] Read current task from `docs/context/CURRENT_TASK.md`
2. [ ] Check related GitHub issue for full context
3. [ ] Verify dependencies are complete
4. [ ] Understand acceptance criteria

## Implementation Protocol

### Phase 1: Explore (Don't code yet!)

- Read all files mentioned in task
- Understand existing patterns
- Identify integration points
- List potential edge cases

### Phase 2: Plan (Still no code!)

- Outline exact changes needed
- List files to create/modify
- Define test cases
- Estimate if task is truly atomic (< 50 LOC)

### Phase 3: Implement

- Write tests first (TDD)
- Implement minimal solution
- Run tests continuously
- Lint after each file change

### Phase 4: Verify & Commit

```bash
# Run all checks
npm test
npm run lint

# Stage and commit
git add -A
git commit -m "feat(module): implement feature (#123)"

# Update GitHub
gh issue comment 123 --body "Implementation complete. PR incoming."
git push -u origin HEAD
gh pr create --title "feat(module): implement feature" --body "Closes #123"
```

## Context Update After Implementation

Update `docs/context/CURRENT_TASK.md`:

```markdown
## Completed: [Task Name]

- Files changed: [list]
- Key decisions: [brief]
- Tests added: [list]
- Next task: [from queue]
```
