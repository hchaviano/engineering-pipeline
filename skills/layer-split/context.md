<!--
Fill when adopting. Leave no <placeholders>.
layer-split uses these layer ids, branch names, and PR conventions — not SKILL.md.
-->
# Layer Split — project context

## Layers (order, only as applicable)

`<e.g. data → repository → bloc → ui>`

Substantial work examples (2+ of these → split): `<e.g. new package/client, repository with real logic, new Bloc, multi-screen UI>`

## Types

`<e.g. feature | bugfix | chore>`

## Base branch

- Default: `<e.g. develop>`
- Release line: `<e.g. release/* — confirm which>`

## Branch naming

Pattern: `<e.g. {type}/{task-number}-{short-desc}-{layer}>`

Example: `<e.g. feature/62310-qr-limits-data → …-repository → …-bloc → …-ui>`

## PRs (`gh pr create`)

- Title: `<e.g. [{Feature|Fix|Chore}] - {Description} (layer woven in)>`
- Body: fill `<path e.g. .github/PULL_REQUEST_TEMPLATE.md>`
- Unmerged bases → create as **draft**

## Commits

`<e.g. Conventional feat:/fix:/chore:>`. Multiple commits per layer are fine.

## Gates

1. Show the full table → wait before creating branches
2. Confirm before push / `gh pr create`
3. Before each layer commit: do not run `verify` here — that layer’s pipeline Quality Gate owns it; 100% of that layer’s new/modified testable logic is tested in the same PR
