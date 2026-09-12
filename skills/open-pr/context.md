<!--
Fill when adopting. Leave no <placeholders>.
open-pr uses these naming, PR, and codegen rules — not SKILL.md.
-->
# Open PR — project context

## Types

`<e.g. feature | fix | chore>`

## Base branch

- Default: `<e.g. develop>`
- Release line: `<e.g. release/* — confirm which>`

## Branch naming

Pattern: `<e.g. {type}/{ticket}-{short-desc}-{part}>` (omit `-{part}` if single)

Layer ids: same as `layer-split` context (`<e.g. data, repository, bloc, ui>`).

## Codegen

If the part adds/changes generated sources: `<command e.g. build_runner build --delete-conflicting-outputs, per touched package>`

Triggers: `<e.g. @JsonSerializable / part '*.g.dart'>`

## Commits

`<e.g. Conventional feat:/fix:/chore:>`

## PRs (`gh pr create`)

- Title: `<e.g. [{Feature|Fix|Chore}] - {título} — verb-led, no ticket number, weave part/layer when split>`
- Body: fill `<path e.g. .github/PULL_REQUEST_TEMPLATE.md>`
  - Description — part contents; full chain when split. On **single or last** PR: ACs, AC→test matrix, coverage %, self-review verdict
  - Validation — how verified; E2E steps on single/last
  - Type of change — check the matching box
- **Ready for review** (never draft)

## Confirmation copy

Ask once: confirm first branch points at `{base}` and the rest stays stacked — then push and open PRs.

## Close out

Delete `.cursor/artifacts/SPEC-{task}.md` and `REVIEW-{task}.md` after every PR exists.
