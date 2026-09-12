---
name: architect
description: Blocking acceptance check — AC↔test matrix, measured coverage, AC-boundary mutation. Use as the Accept stage of engineering-pipeline, after self-review.
---

# Architect

Stand-in for a mutation pass. Unattended, blocking, never deferred. How this project mutates (test-boundary vs a mutation tool) is in [context.md](context.md).

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask.

## Execution

Skills cannot pin the chat model. **Parent agents** must run this via `.cursor/agents/architect.md`. If you **are** already that agent, run the three checks below — do not re-launch yourself.

Need: path to `SPEC-{task}.md` (`status: implemented` or still `approved` with Traceability filled) and the diff range.

## Checks (all must pass)

**(a) Matrix** — every AC maps to ≥1 test that *asserts* the Then, not just exercises it. An AC without one blocks. Do not edit ACs to match the code.

**(b) Coverage** — run the coverage command in [context.md](context.md). Report the real percentage for new/modified files. A file below the bar on testable logic blocks, with uncovered lines listed. Thin passthrough already declared untestable by the Coder is exempt if still zero-logic.

**(c) AC mutation** — for each AC on a critical flow ([context.md](context.md)) that states an explicit boundary/condition, follow the mutation method in that file. **Always revert** temporary edits. Never commit a mutant.

Non-critical ACs: mutate when the boundary is non-trivial; skip obvious tautologies.

## Done

Return: matrix completeness, coverage %, mutation results (which ACs mutated, any survivors). Block ship on any failure. Do not mark the spec `verified` — that happens after `verify`.
