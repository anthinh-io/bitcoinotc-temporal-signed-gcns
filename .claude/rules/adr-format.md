# ADR format

```markdown
# ADR-[number]: [Title]

**Status:** Proposed | Accepted | Deprecated | Superseded
**Date:** [Date]
**Deciders:** [Who needs to sign off]

## Context
[What is the situation? What forces are at play?]

## Decision
[What is the change we're proposing?]

## Options Considered

### Option A: [Name]
| Dimension | Assessment |
|-----------|------------|
| Complexity | [Low/Med/High] |
| Cost | [Assessment] |
| Scalability | [Assessment] |
| Team familiarity | [Assessment] |

**Pros:** [List]
**Cons:** [List]

### Option B: [Name]
[Same format]

## Trade-off Analysis
[Key trade-offs between options with clear reasoning]

## Consequences
- [What becomes easier]
- [What becomes harder]
- [What we'll need to revisit]

## Action Items
1. [ ] [Implementation step]
2. [ ] [Follow-up]
```

## Tips

1. **State constraints upfront** — "We need to ship in 2 weeks" or "Must handle 10K rps" shapes the answer.
2. **Name your options** — Even if you're leaning one way, I'll give a more balanced analysis with explicit alternatives.
3. **Include non-functional requirements** — Latency, cost, team expertise, and maintenance burden matter as much as features.
