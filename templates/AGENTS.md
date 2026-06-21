## Agent operating baseline

This file is provided by `@marcelle-labs/agent-substrate`. It establishes the
shared baseline every Marcelle Labs repo's agents operate under. Repo-specific
guidance belongs in this repo's own docs, not here.

### Ground rules

- Work from evidence. Verify claims against code, tests, or tool output before
  asserting them.
- Prefer the smallest change that satisfies the request. Do not expand scope.
- Leave the workspace clean: no stray temp files, no commented-out dead code.

### Model routing

Model selection is governed by the substrate's `BudgetController`, which reads
`routing/model-routing-table.json`. Start on the cheapest model that clears the
task's success threshold; escalate only when the policy says to.

### Updating this file

Do not hand-edit this generated file. Edit the canonical source in the substrate
(`templates/AGENTS.md`) or override it locally via this repo's `.agents/AGENTS.md`,
then re-run `agent-substrate sync`.
