---
name: self-review
description: Isolated pre-PR review of a diff against the project review rubric. Review only — never fixes code. Use before push or as the Self Review pipeline stage.
---

# Self Review

Strict review of the current working diff, before pushing. Review only — this skill never edits code; fixes are a separate, explicit step.

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask.

## Scope

The current diff against the base branch (or the range the user specifies). In a `layer-split` chain the base is the **previous layer’s branch**, not the repo default — state the range in the subagent prompt. `.cursor/artifacts/` is shared across worktrees (symlink), so the REVIEW artifact is visible to every session.

## Execution

Skills cannot pin the chat model. **Parent agents** must run this via `.cursor/agents/self-review.md` (`readonly: true`). If you **are** already that agent, review and write the artifact — do not re-launch yourself.

Parents: launch an **isolated** Task with the `self-review` agent (do **not** resume this conversation). Prompt must include:

1. The diff range
2. Path to `.cursor/artifacts/SPEC-{task}.md` if one exists
3. Instruction to apply the review rubric named in [context.md](context.md) **as-is** — do not invent another rubric. For review-lessons nits named there, also read the catalog.

Do not include this conversation’s history, the approved plan, or any rationale already discussed. The subagent may read the repo; what’s withheld is this session’s history.

## Criteria

The review-instructions file as-is (severities, Always check, Do not report, nit cap, `file:line` evidence). If a spec exists: flag AC gaps (diff does something unspecified, or AC uncovered).

Surface first the priority checks already listed in that rubric (security, layer boundaries, unreported errors, missing tests, …). Do not add a parallel checklist here.

## Output

Persist `.cursor/artifacts/REVIEW-{task}.md`: severity-ordered list + verdict `"ready to push"` or `"fix N items first"`.

**Any new/modified testable unit without a test is always 🔴 Important and always forces "fix N items first"** (coverage bar in the standards file named in [context.md](context.md)).

The verdict is copied into the PR body later; `open-pr` deletes the artifact at task close. Fixing accepted findings is a separate step.
