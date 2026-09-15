<!--
Fill when adopting. Leave no <placeholders>.
The orchestrator reads this file. Product, stack, commands, and model pins belong here — not in SKILL.md.
-->
# Engineering Pipeline — project context

## Standards

- Engineering: `<path e.g. CLAUDE.md>`
- Review rubric: `<path e.g. REVIEW.md — start from the kit’s abstract REVIEW.md>`
- ADRs (optional): `<path e.g. docs/adr/ or omit>`
- Review-lessons catalog: `.cursor/skills/review-lessons/lessons.md`

## Product

- Name: `<product>`
- Critical flows (extra care; architect mutates these AC boundaries): `<list>`
- Sensitive data (never logs / analytics / insecure storage): `<list>`

## Triviality escape hatch

Collapse Specify into one sentence only when **all** hold:

- Touched files ≤ `<n e.g. 2>`
- No critical flow and no sensitive data (see Product)
- No new testable logic

## Worktrees

- One feature per worktree. Shared `.cursor/` (symlink) is expected so artifacts hand off.
- Keep `SPEC-{task}` slugs unique across concurrent features.

## Agent model pins

Copy these into `.cursor/agents/*.md` (`model:` frontmatter). Adjust per team.

| Skill | Agent model |
|-------|-------------|
| `specifier` | `<e.g. claude-opus-5[effort=high]>` |
| `feature-implementation` | `<e.g. claude-opus-5[effort=high]>` |
| `refactorer` | `<e.g. composer-2.5[fast=false]>` |
| `architect` | `<e.g. composer-2.5-fast>` (standalone Accept; not launched in the default pipeline) |
| `self-review` | `<e.g. claude-opus-5[effort=high]>` (agent `readonly: true`) |
| `debugging` | `<e.g. claude-opus-5[effort=high]>` |
| `layer-split` / `verify` / `open-pr` | `<e.g. composer-2.5-fast>` |
| `engineering-pipeline` | `<orchestrator model>` |

## Uncle Bob stand-ins (this project)

| Uncle Bob | Here |
|-----------|------|
| Gherkin / `.feature` files | `<e.g. numbered Given/When/Then ACs in the SPEC>` |
| Mutation testing (Stryker / etc.) | `<e.g. flip AC boundary in the mapped test, run it, revert>` |
| Coverage gate | `<command e.g. make coverage>` — collect in the same `verify` test run when Acceptance Check is on |
