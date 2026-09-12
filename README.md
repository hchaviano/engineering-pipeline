# Engineering Pipeline

Informal request → frozen SPEC → tests-first code → optional refactor → isolated review → acceptance → quality gate → PR.

Human attention is front-loaded (Challenge + Plan). The Coder, Refactorer, and Architect never see the informal notes — only the approved SPEC and the diff. Adapted from Uncle Bob’s [agent transformation pipeline](https://medium.com/@adrianbailador/uncle-bobs-agent-pipeline-from-informal-specs-to-mutation-tested-net-code-ac2baa45cfd5); stack-specific stand-ins live in each skill’s `context.md`, not in the process.

```text
 ticket / chat
       │
       ▼
 ┌─────────────┐   Challenge / Plan stop gates    ┌──────────────┐
 │  Specify    │ ───────────────────────────────► │ SPEC approved│
 └─────────────┘                                  └──────┬───────┘
                                                         │
                    layer-split (if the plan says so)    │
                                                         ▼
 ┌─────────────┐  SPEC path only; tests first     ┌──────────────┐
 │    Code     │ ───────────────────────────────► │ tests green  │
 └─────────────┘                                  └──────┬───────┘
                                                         │
              refactorer if complexity bar trips         │
                                                         ▼
 ┌─────────────┐  isolated, REVIEW.md as-is       ┌──────────────┐
 │ Self Review │ ───────────────────────────────► │ REVIEW artifact
 └─────────────┘                                  └──────┬───────┘
                                                         ▼
 ┌─────────────┐  AC↔test + coverage + mutation   ┌──────────────┐
 │   Accept    │ ───────────────────────────────► │ architect OK │
 └─────────────┘                                  └──────┬───────┘
                                                         ▼
 ┌─────────────┐                                      ┌──────────────┐
 │   Verify    │ ───────────────────────────────────► │ spec verified│
 └─────────────┘                                      └──────┬───────┘
                                                             ▼
                                                        Ship (PR)
```

## Start

| Situation | Invoke |
|-----------|--------|
| Any non-trivial ticket | `/engineering-pipeline` |
| Bug, crash, unexpected behavior | `/debugging` — do not start on the pipeline |
| Format / analyze / test only | `/verify` |
| Finished, verified diff → PR | `/open-pr` |

Parents launch the matching agent in `agents/` (skills cannot pin models). Do not run sibling stages inline on the orchestrator.

**Triviality:** collapse Specify into one sentence only when the bars in `skills/engineering-pipeline/context.md` all hold (file count, no critical flow / sensitive data, no new testable logic). State why in one line; otherwise run the full pipeline.

## Workflow

Each stage is a sibling skill. The orchestrator (`engineering-pipeline`) launches that skill’s **agent**; it does not re-derive the rules.

### 1. Specify — `specifier`

Informal request → hard spec. The specifier never writes production code.

1. **Understand** — ticket, affected code, critical flows / sensitive data, review-lessons. 3–5 line summary.
2. **Challenge** — already exists? architecture break? simpler / right scope? **Stop gate:** if objections, present them and wait. Record objection → decision in `## Decisions`.
3. **Specification** — copy `artifacts/SPEC-template.md` → `artifacts/SPEC-{task}.md` (`status: draft`). Numbered Given/When/Then ACs (`AC-1`, …). Explicit `Out of scope`.
4. **Architecture Validation** — map ACs to layers, dependency direction, packages touched.
5. **Plan** — ordered file changes; whether `layer-split` applies. **Stop gate:** present the plan and wait. On approval, `status: approved` — **frozen**. Changing an AC later is a scope change and needs the same approval.

Skip this stage for trivial changes and for bugs already handled by `debugging`.

### 2. Layer split (if applicable) — `layer-split`

If the approved plan included a split, create the stacked branches **before any code**. Later stages run per layer, on that layer’s branch. Parallelism is per **feature** (one worktree), not per layer.

Small features stay a single branch. Never force the split.

### 3. Code — `feature-implementation`

The Coder sees **only**: path to the approved SPEC, the plan file list, and the layer in progress.

Do **not** forward the informal request, ticket chatter, or Challenge debate. If the SPEC is silent or contradictory, the Coder asks — it does not guess. Never edit an approved AC to match what got built.

Order is fixed: failing acceptance tests from ACs → unit tests for internals → minimum implementation (layer by layer) → fill `## Traceability` (`AC-id → test::group`). Hand off when the new tests are green.

### 4. Refactor (conditional) — `refactorer`

Runs when the diff trips the complexity bar in that skill’s `context.md` (size / nesting / duplication) or the Coder flags it. Otherwise skip.

No product requirements. Behavior must not change. Tests stay green. Stays on the current layer branch.

### 5. Self review — `self-review`

Isolated, **readonly**, no parent history. Rubric is [`REVIEW.md`](REVIEW.md) as-is — do not invent another.

Prompt is the diff range + SPEC path if any. Output: `artifacts/REVIEW-{task}.md` with a severity-ordered list and verdict `"ready to push"` or `"fix N items first"`.

Any new/modified testable unit without a test is 🔴 Important and forces `"fix N items first"`. Fixes are a separate, explicit step.

### 6. Accept — `architect`

Blocking; never deferred.

- **Matrix** — every AC has ≥1 test that *asserts* the Then.
- **Coverage** — measured on new/modified testable logic; below the bar blocks.
- **AC mutation** — on critical-flow boundaries, flip the condition in the mapped test (or run the mutation tool named in context). If the mutant still passes, the test is fake. **Always revert.** Never commit a mutant.

Do not mark the spec `verified` here.

### 7. Quality gate — `verify`

Pinned toolchain, touched paths only. Format → analyze → test, in that order. Failures get fixed and re-run; the change is not done while a required gate is red.

On pass: spec `status: verified`. Prepare ACs + traceability + coverage % + self-review verdict for the PR body. Fold architectural `## Decisions` into ADRs if context names a location.

### 8. Ship — `open-pr`

Confirm once, then push and open PR(s). If stage 2 already stacked branches, push that stack — don’t re-split.

After every PR exists: delete `SPEC-{task}.md` and `REVIEW-{task}.md`.

## Isolation (non-negotiable)

| Role | Sees | Must not see |
|------|------|----------------|
| Specifier | ticket, codebase, review-lessons | — (produces the SPEC) |
| Coder | approved SPEC + plan files + layer | informal request, Challenge debate |
| Refactorer | diff / files (SPEC only to avoid behavior change) | ticket text |
| Self-review | diff + SPEC artifact + `REVIEW.md` | parent conversation, plan rationale |
| Architect | SPEC + diff + coverage/mutation commands | informal notes |

## Bugs

`/debugging` first: reproduce → isolate through the layers → validated hypothesis → propose. **Stop gate:** if the user only reported the bug, deliver diagnosis and wait.

Apply the fix only when asked. A regression test must fail without the fix and pass with it, then `/verify`. If the fix spans 2+ layers or changes specified behavior, hand off to `/engineering-pipeline` at Specify — do not drive-by patch.

## Standing (any stage)

After applying an **accurate** PR comment or a user correction to a pipeline decision, harvest one reusable Do/Don't into `skills/review-lessons/lessons.md` before considering the pass done. Skip dismissed comments and typo-only nits. Consult the catalog at Specify and Code.

Worktrees: one feature per worktree (`worktree` skill). Shared `.cursor/` is expected so artifacts hand off. Never `git stash` across worktrees (`refs/stash` is repo-wide). Keep `SPEC-{task}` slugs unique.

## Uncle Bob → here

The article is a .NET pipeline (Gherkin + Reqnroll + Stryker). Same stages and isolation; the stand-ins are in `context.md`.

| Uncle Bob | This workflow | Fill in `context.md` |
|-----------|---------------|----------------------|
| Informal specs (you) | Ticket / chat | — |
| Specifier → hard specs | `specifier` → `SPEC-{task}.md` | AC format, SPEC template |
| Specifier → Gherkin | Numbered Given/When/Then ACs | `.feature` files if you use them |
| Coder (no informal notes) | `feature-implementation`, SPEC-only | test-first order, layer order, coverage bar |
| Refactorer (no requirements) | `refactorer` | size/nesting triggers |
| Architect → mutation | `architect` — matrix + coverage + AC-boundary mutation | coverage command; test-boundary vs Stryker/etc. |

## Repo layout

```text
.
├── README.md            # this workflow
├── REVIEW.md            # abstract review rubric (self-review applies it as-is)
├── agents/              # thin wrappers: model pin + “follow the skill”
├── skills/              # process (SKILL.md) + per-skill context.md
├── artifacts/           # SPEC-template.md; runtime SPEC-{task}.md / REVIEW-{task}.md
├── rules/               # always-on: apply / harvest review-lessons
└── scripts/             # worktree create / remove (optional)
```

```text
skills/<name>/
├── SKILL.md      # process — what the stage does
└── context.md    # project fill-in — stack, commands, standards, conventions
```

Skills own **process**. They read `context.md` and stop if `<placeholders>` remain. Do not bake product or framework details into `SKILL.md`. `self-review` uses [`REVIEW.md`](REVIEW.md); that file only tunes severity, scope, always-flag items, and volume — full coding standards stay in the file it names (e.g. `CLAUDE.md`).

## Wire to a project

1. Copy `agents/`, `skills/`, `artifacts/`, `rules/`, `scripts/` into `<project>/.cursor/`. Copy `REVIEW.md` to the project root (or the path in `self-review/context.md`).
2. Fill every `skills/*/context.md` and `REVIEW.md` (search for `<placeholder>`). Leave no angle-bracket tokens.
3. Pin `model:` in `agents/*.md` to match `skills/engineering-pipeline/context.md`.
4. Point context files at your standards (`CLAUDE.md`, `REVIEW.md`, or equivalents).
