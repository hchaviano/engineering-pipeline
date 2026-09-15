---
name: feature-implementation
description: Coder — tests-first implementation against an approved SPEC only. Never sees informal notes. Use during the Code stage of engineering-pipeline.
---

# Feature Implementation (Coder)

Constraints while coding against an already-approved SPEC. Not a pipeline — see `engineering-pipeline`.

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask.

## Execution

Skills cannot pin the chat model. **Parent agents** must run this via `.cursor/agents/feature-implementation.md`. If you **are** already that agent, implement per the rules below — do not re-launch yourself.

Parent prompt **must** contain only: path to approved `SPEC-{task}.md`, **current Implementation Phase**, layer in progress. If informal notes or Challenge debate arrived in the prompt, ignore them — if an AC is missing, stop and ask.

## Isolation rule

You never saw the informal request. If the SPEC is silent, **ask** — do not invent behavior. Never edit an approved AC to match what got built.

## Tests first, then code (fixed order)

1. **Acceptance tests from ACs** — one test per AC that *asserts* the Then (not just executes it). They fail. Name the group with the AC-id. Test kinds are in [context.md](context.md).
2. **Unit tests** — internals the ACs don’t reach (mappers, validators, edge values).
3. **Implementation** — minimum code to make them pass. Layer order is in [context.md](context.md). Each layer compiles and is tested before the next.
4. Fill spec `## Traceability` (`AC-id → test file::group`) as tests land.

Hand off only when the new tests are green. Set the current phase’s **Status:** `done`. Do **not** launch `verify` — the parent’s Quality Gate is the one mechanical pass. Do not pull the next phase forward.

Also apply the review-lessons catalog (conventions, not product scope).

## Scope

- Don’t fix adjacent bugs or refactor beyond the feature without flagging. Report pre-existing bugs and continue unless they block.
- Opportunistic migration: if you touch legacy listed in [context.md](context.md), migrate only the part the change already touches.
- Coverage bar in [context.md](context.md) is mandatory. Genuinely untestable (thin passthrough, zero logic): say so explicitly.

## Stop and ask

- Product ambiguity (AC missing or contradictory)
- Backend contract / field that doesn’t exist
- UI that belongs in the shared design-system package instead of the feature
- Any decision that would change approved scope
- Extra stop cases listed in [context.md](context.md)

## Reference

Standards file and review-lessons path: [context.md](context.md).
