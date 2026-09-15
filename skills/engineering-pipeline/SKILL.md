---
name: engineering-pipeline
description: End-to-end engineering workflow — informal request to mutation-checked, reviewed, shipped code. Use when starting any ticket or substantial change.
---

# Engineering Pipeline

Orchestrator. Sibling skills own their criteria — launch their **agents** in `.cursor/agents/`; do not re-derive rules here.

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask — do not invent stack, commands, or product rules.

## Execution

Skills cannot pin the chat model. **Parent agents** must run this skill via `.cursor/agents/engineering-pipeline.md`. If you **are** already that agent, execute the stages below — do not re-launch yourself.

When `/engineering-pipeline` is invoked (parent only):
1. Launch Task with the `engineering-pipeline` agent.
2. Pass a self-contained prompt: ticket/goal, known constraints, artifact paths.
3. Every sibling stage launches that sibling’s agent — never inline `verify` / `open-pr` / etc.

**Worktrees:** one feature per worktree (`worktree` skill). Artifact names are per-task — keep slugs unique.

**Triviality escape hatch:** collapse specifier stages into one sentence only when **all** hold: the file-count and risk bars in [context.md](context.md). State why in one line; otherwise full pipeline.

**Review lessons:** catalog path is in [context.md](context.md). Consult it at Specify and Code. After applying an accurate PR comment or user correction, harvest per the `review-lessons` skill.

## Cost (keep the pipeline, shrink the bill)

Stop gates, SPEC isolation, expensive specifier/coder/review, and blocking Accept stay. What changes is spawn count:

- **This orchestrator does not run gates inline.** Sibling agents own them — but **once**. Do not re-Read the standards file named in [context.md](context.md) (already in context). Prompt siblings with SPEC path + diff range + task slug only — never the informal request or this transcript.
- **One mechanical `verify`:** Quality Gate includes Acceptance Check (matrix, coverage, mutation). Do **not** also launch `architect`. Do not let Code, Refactor, layer-split, or open-pr spawn `verify`.
- **Standalone `/architect`** still exists for Accept-only reruns; the default pipeline does not stack it on top of verify.
- **Session:** one ticket per chat. After Ship, start a new conversation before the next ticket.

## Stages

1. **Specify** — launch `specifier`. Human-heavy. Stop gates (Challenge, Plan) return to the user. Output: `SPEC-{task}.md` with `status: approved` (frozen). Skip for trivial changes and for bugs already handled by `debugging`.

2. **Layer Split (if applicable)** — if the approved plan included a split, launch `layer-split` **before any code**. Later stages run per layer, on that layer’s branch.

3. **Code** — launch `feature-implementation` (the Coder). Prompt = approved SPEC path + plan file list + layer in progress. The Coder also reads review-lessons. **Do not forward the informal request, ticket chatter, or Challenge debate.** The Coder must not fill gaps from context it was never given. If the SPEC is ambiguous, it asks — it does not guess. The Coder must **not** launch `verify`.

4. **Refactor (conditional)** — launch `refactorer` when the diff trips the complexity bar in that skill’s context, or the Coder flags it. Otherwise skip. Stays on the current layer branch. Prompt = diff range only (no informal specs). The Refactorer must **not** launch `verify` — Quality Gate is the one mechanical pass.

5. **Self Review** — launch `self-review` (isolated, readonly). No parent history. Rubric is the review-instructions file named in [context.md](context.md).

6. **Quality Gate (blocking)** — launch `verify` **once**, with the SPEC path and the instruction to run the gate **including Acceptance Check** (AC matrix, measured coverage, AC mutation on critical-flow boundaries). Never defer gaps. On pass: spec `verified`; prepare ACs + traceability + coverage % + self-review verdict for the PR body; fold architectural `## Decisions` into the ADR location in [context.md](context.md) if one is set. Do **not** launch `architect` in this default path.

7. **Ship** — launch `open-pr`. Deletes SPEC/REVIEW artifacts after PRs exist. If stage 2 already stacked branches, push that stack — don’t re-split. Do not launch `verify` from Ship.

**Standing (any stage):** after applying an accurate PR review comment or a user correction to a pipeline decision, harvest it into `review-lessons` before considering the pass done.

## Sibling agents

Model pins live in [context.md](context.md) and in `.cursor/agents/*.md`. Do not invent pins.
