# Active Blockers

<!-- Items here require HUMAN intervention -->
<!-- Clear each item immediately after it's resolved -->
<!-- If this file is empty, there are no blockers -->

## Status: ✅ No Active Blockers

---

## When a Blocker is Added

Template for new blockers:

```markdown
## 🛑 [BLOCKER-ID]: [Brief Title]

**Added**: [date]
**Blocking**: #[issue-number] - [task name]
**Type**: [database | api-key | mcp-setup | oauth | deployment | other]

### What's Needed

[Specific thing the human needs to do]

### Instructions for Human

1. [Step 1]
2. [Step 2]
3. [Step 3]

### After Resolving

- Delete this blocker entry
- Tell Claude: "Blocker [ID] resolved, continue with #[issue]"
- Or run: `/project:resume-task [issue-number]`

### Meanwhile

Claude can work on: #[alternative-issue] (no dependency on this)
```

---

## Blocker Types Quick Reference

| Type               | Common Causes                              | Who Resolves |
| ------------------ | ------------------------------------------ | ------------ |
| `database`         | Connection string, migrations, permissions | DevOps/Human |
| `api-key`          | Missing secrets, expired tokens            | Human        |
| `mcp-setup`        | Server not configured, auth needed         | Human        |
| `oauth`            | App registration, callback URLs            | Human        |
| `deployment`       | CI/CD, hosting config, DNS                 | DevOps/Human |
| `external-service` | Third-party signup, approval               | Human        |
| `hardware`         | Device access, local setup                 | Human        |

---

_This file should usually be empty or very short. If it grows large, the project has dependency problems._
