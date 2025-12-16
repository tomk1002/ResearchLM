# Context Management Rules

<!-- This is the SINGLE SOURCE OF TRUTH for context management -->
<!-- All agents reference this file - do not duplicate these rules elsewhere -->

## Prime Directive

> **Optimize context usage by prioritizing relevance, summarizing aggressively, isolating agent memory, and pruning stale or low-signal information to prevent context rot.**

---

## Context File Structure

```
docs/
├── context/                    # ACTIVE context (keep small!)
│   ├── _CONTEXT_RULES.md      # This file - rules for context management
│   ├── CURRENT_TASK.md        # Current task ONLY (replace each task)
│   ├── ARCHITECTURE.md        # High-level system design (update rarely)
│   ├── DECISIONS.md           # Key decisions log (append-only)
│   └── BLOCKERS.md            # Human intervention needed (see protocol)
├── planning/                   # Planning artifacts
│   ├── MASTER_PLAN.md         # Epic/feature breakdown
│   └── TASK_QUEUE.md          # Ordered task list
└── archives/                   # Historical context (date-organized)
    └── YYYY-MM-DD/
        └── [archived-files].md
```

---

## Context Budget

| File             | Max Lines | Update Frequency               |
| ---------------- | --------- | ------------------------------ |
| CURRENT_TASK.md  | 100       | Replace each task              |
| ARCHITECTURE.md  | 200       | When design changes            |
| DECISIONS.md     | 300       | Append only, summarize monthly |
| BLOCKERS.md      | 50        | Clear when resolved            |
| **Total Active** | **~650**  | —                              |

If total exceeds 800 lines → run `/project:prune-context` immediately.

---

## Context Rotation Rules

### CURRENT_TASK.md

- **Replace entirely** when starting new task
- Contains ONLY: current issue, acceptance criteria, progress notes
- Never accumulate multiple tasks

### ARCHITECTURE.md

- **Update only** when system design fundamentally changes
- Keep high-level (no implementation details)
- Reference files instead of including code: "see `src/auth/jwt.ts`"

### DECISIONS.md

- **Append only** - never delete entries
- Summarize to ~100 lines monthly (keep last 3 months detailed)
- Format: Date, Decision, Reason, Alternatives, Consequences

### BLOCKERS.md

- **Clear immediately** when blocker is resolved
- Contains ONLY active blockers requiring human intervention
- Empty file = no blockers

---

## What Belongs in Context vs. Archives

### KEEP in Active Context

- Current task details
- Decisions affecting next 3 tasks
- Active blockers
- Architecture constraints still in effect
- API contracts currently being used

### MOVE to Archives

- Completed task details
- Exploration notes for finished work
- Superseded decisions
- Old debugging sessions
- Implementation details of merged code

### DELETE Entirely

- Temporary debugging output
- Duplicate information
- Resolved discussions
- Outdated alternatives

---

## Summarization Techniques

### Replace Code with References

````markdown
# Before (wastes context)

Here's the auth implementation:

```javascript
export async function validateToken(token) {
  const decoded = jwt.verify(token, process.env.SECRET);
  // ... 50 more lines
}
```
````

# After (preserves context)

Auth uses JWT validation - see `src/auth/validate.ts:15-65`

````

### Condense Explanations
```markdown
# Before
We considered using sessions but decided against it because
sessions require server-side storage which adds complexity
and doesn't scale well horizontally. We also looked at...

# After
**Auth approach**: JWT (stateless, scales horizontally)
- Rejected: sessions (server storage complexity)
````

### Use Tables for Comparisons

```markdown
# Before

Option A has better performance but worse DX. Option B is
easier to use but slower. Option C is balanced but requires...

# After

| Option | Performance | DX     | Chosen |
| ------ | ----------- | ------ | ------ |
| A      | ⭐⭐⭐      | ⭐     | No     |
| B      | ⭐          | ⭐⭐⭐ | No     |
| C      | ⭐⭐        | ⭐⭐   | ✅     |
```

---

## Context Health Checks

Run these checks weekly or when Claude seems "confused":

```bash
# Check total context size
wc -l docs/context/*.md

# Find largest files
ls -lS docs/context/

# Check for stale content (not modified in 7 days)
find docs/context -name "*.md" -mtime +7
```

### Signs of Context Rot

- Claude repeats itself or forgets recent decisions
- Responses become inconsistent
- Claude asks about things already documented
- Total context exceeds 800 lines

### Recovery Protocol

1. Run `/project:prune-context`
2. Use `/clear` in Claude Code
3. Re-read only essential context files
4. If severe: archive everything, rebuild from GitHub issues

---

## Agent Context Loading

When starting a task, agents should load context in this order:

1. **Always**: `_CONTEXT_RULES.md` (this file)
2. **Always**: `CURRENT_TASK.md`
3. **If exists**: `BLOCKERS.md`
4. **As needed**: `ARCHITECTURE.md` (for structural decisions)
5. **As needed**: `DECISIONS.md` (for relevant past decisions)
6. **Never**: Archives (unless explicitly searching for something)
