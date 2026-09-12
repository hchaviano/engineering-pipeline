<!--
Fill when adopting. Leave no <placeholders>.
The coder reads this file. Test stack, layer order, and coverage belong here — not in SKILL.md.
-->
# Feature Implementation — project context

## Standards

- Engineering: `<path e.g. CLAUDE.md>`
- Review-lessons catalog: `.cursor/skills/review-lessons/lessons.md`

## Tests first

- Acceptance tests: `<e.g. bloc_test / widget / unit — one asserting test per AC>`
- Test helpers: `<e.g. pumpApp, private mocks per file, mock library>`
- Name groups with the AC-id.

## Implementation order

Layers, only as applicable: `<e.g. Data → Repository → Bloc/Cubit → Presentation>`

Each layer compiles and is tested before the next.

## Coverage

- Bar: `<e.g. 100% of new/modified testable logic>`
- Command: `<e.g. make coverage>`
- Untestable exemption: thin passthrough, zero logic — declare it explicitly.

## Legacy / opportunistic migration

When the change already touches these, migrate that part only:

- `<legacy pattern → target e.g. freezed → Equatable + manual copyWith>`
- `<legacy pattern → target>`

## Design system

- Shared UI lives in: `<package or folder>`
- Feature `widgets/` is composition only. If a widget belongs in the kit, stop and ask.

## Extra stop-and-ask cases

- `<e.g. BFF endpoint/field missing>`
- `<or delete this section>`
