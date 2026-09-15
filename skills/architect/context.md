<!--
Fill when adopting. Leave no <placeholders>.
**Default pipeline:** `engineering-pipeline` does not launch this agent. Fill this file anyway — `verify` Acceptance Check uses the same coverage command and mutation method.
-->
# Architect — project context

## Inputs

- SPEC path: `.cursor/artifacts/SPEC-{task}.md`
- Critical flows (mutate these AC boundaries): `<list — same as engineering-pipeline/context.md>`

## Coverage

- Command: `<e.g. make coverage, npm test -- --coverage, scoped to the touched package>`
- Bar: `<e.g. 100% of new/modified testable logic>`
- SDK / toolchain pin: `<e.g. fvm-pinned Flutter, nvm, or omit>`

## Mutation method

Pick one and delete the other.

### Test-boundary (no mutation tool)

For each mapped AC with an explicit boundary:

1. From `## Traceability`, open the mapped test.
2. Temporarily flip the boundary in that test’s input/expectation (e.g. `>100` tried as `99` and `101`).
3. Run `<test command e.g. fvm flutter test <file>>`.
4. If it still **passes**, the test does not actually exercise the AC — fix the assert (or tell the parent the Coder must). If it **fails**, the AC is real.
5. **Always revert** the temporary edit.

### Mutation tool

- Tool: `<e.g. Stryker, mutation_test>`
- Command: `<command>`
- Survivors on AC-mapped tests block.

## Exempt

Thin passthrough already declared untestable by the Coder, if still zero-logic.
