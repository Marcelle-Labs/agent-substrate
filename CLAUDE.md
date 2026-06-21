# CLAUDE.md — agent-substrate

Instructions for agents working **on** the agent-substrate package itself.

## Project

`@marcelle-labs/agent-substrate` is the shared agent operating substrate: canonical
agent config, skills, templates, and the model-routing policy. It is published to
npm and synced into consumer repos via the `agent-substrate sync` CLI (no symlinks,
no Claude Code `extends` dependency).

## Working rules

- **Evidence-gated issues.** Work Linear issues (`VR-###`) in order. Do not start the
  next until the current one is Done with its shell-verifiable gate passing.
- **Comment is the artifact.** When you close an issue, comment with (1) files
  created/modified, (2) the exact shell command proving the gate, (3) the output.
  Future agents read the comments, not the code, to learn how this was built.
- **Commit format:** `feat(scope): description [VR-NNN]`.
- **Generated files** carry the header `# Generated from @marcelle-labs/agent-substrate@{version}`.
  Never hand-edit a generated file in a consumer repo — edit the canonical source here.

## Layout

- `templates/` — canonical per-tool config (source for `sync`).
- `routing/model-routing-table.json` — routing policy (`BudgetController` reads this).
- `bin/agent-substrate-sync.ts` — the sync CLI.
- `skills/`, `.claude/` — published skills and this repo's Claude config.

## Commands

```sh
npm run build         # tsc -> dist/
npm run sync:dry      # preview sync output (no writes)
npx ts-node bin/agent-substrate-sync.ts --dry-run
```
