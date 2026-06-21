# agent-substrate

Canonical agent operating substrate for Marcelle Labs. This package is the single
source of truth for agent configuration that is shared across repos: agent
instructions, skills, templates, and the model-routing policy.

## What lives here

| Path                            | Purpose                                                              |
| ------------------------------- | ------------------------------------------------------------------- |
| `templates/`                    | Canonical per-tool config rendered into consumer repos by `sync`.   |
| `routing/model-routing-table.json` | Cost/success routing policy read by the `BudgetController`.      |
| `skills/`                       | Reusable agent skills published to consumers.                       |
| `.claude/`                      | Claude Code configuration for this repo.                            |
| `bin/`                          | The `agent-substrate` CLI (`sync`).                                 |
| `AGENTS.md` / `CLAUDE.md`       | This repo's own agent instructions.                                 |

## Distribution model

Consumers do **not** symlink into this repo and do **not** rely on the Claude Code
settings `extends` field (not a stable API). Instead they run:

```sh
pnpm dlx @marcelle-labs/agent-substrate sync
```

`sync` renders per-tool config formats from the canonical source and writes them
into the consumer repo. Every generated file carries a checksum header:

```
# Generated from @marcelle-labs/agent-substrate@{version}
```

Use `--dry-run` to preview the file list without writing.

## Build process as reference artifact

This repo is built issue-by-issue (Linear `VR-###`). Each closed issue carries a
comment recording the files changed, the exact shell command that proves the gate,
and the observed output. Read the closed issues to understand how this was built.

Spec: `SOLO-FOUNDER-AGENT-OPS-01`.
