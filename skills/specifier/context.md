<!--
Fill when adopting. Leave no <placeholders>.
The specifier reads this file. Search paths, layers, and AC style belong here — not in SKILL.md.
-->
# Specifier — project context

## Standards

- Engineering: `<path e.g. CLAUDE.md>`
- Review-lessons catalog: `.cursor/skills/review-lessons/lessons.md`
- SPEC template: `.cursor/artifacts/SPEC-template.md`

## Product

- Critical flows: `<list — same as engineering-pipeline/context.md>`
- Sensitive data: `<list>`

## Understand / Challenge

- Search existing similar features in: `<paths e.g. packages/, src/components/>`
- Shared UI / design system: `<package or folder, or omit>`

## Architecture

- Layers (dependency direction, no skipping, no inverse deps): `<e.g. Presentation → Business Logic → Repository → Data>`
- State management: `<e.g. Bloc vs Cubit rules, or Redux vs local state>`
- Routing: `<e.g. go vs push, named routes>`

## Specification

- AC style: numbered Given/When/Then (`AC-1`, …) unless you use `<alternate e.g. Gherkin .feature files>`
- Tests that will consume ACs: `<e.g. unit / widget / API tests — not .feature files>`
- Extra SPEC sections required: `<UI states, errors, l10n, analytics — delete any the project does not use>`

## Plan

- Layer-split bar: see `layer-split` skill context. Name layers with the identifiers that skill uses.
