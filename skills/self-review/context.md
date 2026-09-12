<!--
Fill when adopting. Leave no <placeholders>.
self-review applies the rubric as-is. Do not duplicate Always-check items here — they belong in the review-instructions file.
-->
# Self Review — project context

## Rubric

- Review instructions: `<path e.g. REVIEW.md — start from the kit’s abstract REVIEW.md>`
- Engineering standards: `<path e.g. CLAUDE.md>`
- Review-lessons catalog: `.cursor/skills/review-lessons/lessons.md` (only for nits the rubric already names)

## Diff range

- Default base: `<e.g. develop>`
- In a `layer-split` chain: previous layer’s branch, not the default base.

## Artifact

- Path: `.cursor/artifacts/REVIEW-{task}.md`
- Verdicts: `"ready to push"` | `"fix N items first"`

## Missing tests

New/modified testable logic without a test → 🔴 Important and `"fix N items first"`, matching the coverage bar in the standards file.
