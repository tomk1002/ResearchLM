Optimize context by removing stale information.

## Protocol

1. Review all files in `docs/context/`
2. For each file, evaluate:

   - Is this relevant to next 3 tasks? Keep if yes.
   - Is this a key decision? Move to DECISIONS.md
   - Is this stale? Move to docs/archives/[date]/

3. Summarize archived content in one paragraph
4. Update context file headers with "Last pruned: [date]"

## Aggressive Summarization Rules

- Replace code examples with "see [file:line]"
- Replace long explanations with bullet points
- Remove resolved discussions
- Consolidate duplicate information
