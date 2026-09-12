<!--
Fill when adopting. Leave no <placeholders>.
The refactorer reads this file. Complexity bar and verify hand-off belong here — not in SKILL.md.
-->
# Refactorer — project context

## Triggers (any one is enough)

- Method/function ≳ `<n e.g. 40>` lines in the diff
- Nesting > `<n e.g. 3>`
- Obvious duplication across files
- Coder flagged the code as tangled

## Verify after edits

Launch the `verify` agent. Commands are in that skill’s context.

## Property tests

- Package in this repo: `<name, or none>`
- If none: omit Uncle Bob’s property-test pass; invariants that ACs already state stay as example-based tests. Adding a package needs the dependency justification in `<standards-file>`.
