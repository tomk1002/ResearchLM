Complete current task and prepare for next.

## Protocol

1. Run all tests: `npm test`
2. Run linter: `npm run lint`
3. Commit changes with proper format
4. Push and create PR: `gh pr create --fill`
5. Update GitHub issue with completion comment
6. Archive task-specific context
7. Update `CURRENT_TASK.md` with next task
8. Summarize any key decisions to `DECISIONS.md`

## Context Pruning

After completion, remove from context:

- Implementation details of completed task
- Exploration notes that are now irrelevant
- Any temporary debugging information

Keep in context:

- Decisions that affect future tasks
- Patterns established by this task
- API signatures that other tasks will use
