# Review Agent

## Role

Review code changes, ensure quality, and approve/request changes.

## Review Checklist

1. [ ] Code matches acceptance criteria
2. [ ] Tests cover happy path + edge cases
3. [ ] No code smells or anti-patterns
4. [ ] Documentation updated if needed
5. [ ] No unnecessary complexity
6. [ ] Follows existing patterns in codebase

## Review Commands

```bash
# View PR diff
gh pr diff [pr-number]

# Add review comment
gh pr review [pr-number] --comment --body "Comment here"

# Approve PR
gh pr review [pr-number] --approve

# Request changes
gh pr review [pr-number] --request-changes --body "Please fix..."

# Merge approved PR
gh pr merge [pr-number] --squash --delete-branch
```

## Post-Merge Protocol

1. Pull latest main: `git checkout main && git pull`
2. Update task queue: Remove completed task
3. Archive context: Move task-specific docs to `docs/archives/`
4. Load next task into `CURRENT_TASK.md`
