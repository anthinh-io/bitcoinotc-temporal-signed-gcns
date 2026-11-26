# CLAUDE.md

## Project-Specific Guidelines

- Before any action that changes what other people can see (Issues, Projects, Labels, Milestones, Pull Requests, Comments, Assignments, ...), show the user exactly what you are about to do and wait for approval. These changes are hard to undo and affect what others see.
  - Approval covers only the actions shown. It does not carry over to later or additional actions; if the plan changes, ask again.
  - For several related actions (e.g. creating a set of issues), one confirmation that lists every action is enough.
  - If an action shows content (title, description, comment), show the exact text first.
- Whenever a decision belongs to the human (choosing between options, confirming an assumption, approving a direction), put it through the `AskUserQuestion` tool instead of plain-text questions. For example, during `/grilling` or `/brainstorm`.
  - Present the options with their trade-offs and one recommendation. In every option's description, explain the reasoning: why it is recommended, or the trade-off and why it is not.
  - Before asking, make sure the user understands the concepts involved: explain each unfamiliar concept briefly and map it to this codebase.
  - If the user replies with a question, answer it and stop; ask for the decision again only in a later turn.
  - Open-ended questions that gather facts only the human knows (for example "how do agents look up declarations today?") may stay in plain text, or use the tool's "Other" option for free-form answers.
- For a feature or integration the codebase has not done before, research before building; do not start from scratch.
  - First search GitHub for 2–3 maintained open-source projects that solve the same problem (prefer official samples and widely used repos), and read how they structure it.
  - Summarise what they do differently and which approach you recommend, with links, before writing code.
  - Feed the comparison into the ADR when the choice is architectural.
  - Skip this for changes that follow a pattern already in the codebase.

## Agent skills

### Issue tracker

Issues are tracked in GitHub Issues via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Default five-role vocabulary (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`). See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: root `GLOSSARY.md` + `docs/adr/`. See `docs/agents/domain.md`.
