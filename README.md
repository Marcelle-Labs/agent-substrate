# agent-substrate

**Shared agent operating substrate for Marcelle Labs.**

[![npm](https://img.shields.io/npm/v/%40marcelle-labs%2Fagent-substrate?style=flat-square)](https://www.npmjs.com/package/@marcelle-labs/agent-substrate)

One source of truth for the agent configuration that would otherwise be copy-pasted
across repositories: agent instructions, reusable skills, per-tool config templates and
the model-routing policy.

## The problem it solves

Agent configuration drifts. Each repository ends up with its own slightly different
`AGENTS.md`, its own tool config, its own idea of which model to route a task to — and
a fix applied in one place never reaches the others.

Symlinking into a shared checkout breaks outside a monorepo, and the Claude Code
settings `extends` field is not a stable API to build on. So consumers **sync** instead:

```sh
pnpm dlx @marcelle-labs/agent-substrate sync
```

That renders the canonical templates into the consumer repository as ordinary committed
files. The result is reviewable in a pull request, works offline, and does not depend
on a shared filesystem layout.

## What lives here

| Path | Purpose |
| --- | --- |
| `templates/` | Canonical per-tool config rendered into consumer repos by `sync` |
| `routing/model-routing-table.json` | Cost and success-rate routing policy, with a JSON Schema beside it |
| `skills/` | Reusable agent skills published to consumers |
| `bin/` | The `agent-substrate` CLI |
| `AGENTS.md` / `CLAUDE.md` | This repository's own agent instructions |

[`AGENTS.md`](./AGENTS.md) is the detailed reference: distribution model, what consumers
may and may not override, and how a template change reaches them.

## Boundary

Internal Marcelle Labs tooling, published so consumer repositories can install it. It is
not a general-purpose agent framework, it makes no claim to work outside the tool
configurations under `templates/`, and it is `UNLICENSED` — published for use, not
offered for redistribution.
