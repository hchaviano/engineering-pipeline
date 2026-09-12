---
name: review-lessons
description: Living memory of accurate PR-review and user corrections. Use when specifying, coding, testing, self-reviewing, or after applying a correct PR comment / user correction — harvest a reusable rule so the same mistake is not repeated.
---

# Review lessons

Catalog: [lessons.md](lessons.md). This skill is **process**; the catalog is **decisions**. Project ids and promotion targets: [context.md](context.md).

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask.

## Apply (every pipeline run)

Read [lessons.md](lessons.md) before the stages in [context.md](context.md). Do not copy the whole catalog into the SPEC. Follow it while writing.

## Harvest (after a correct comment)

Trigger: an **accurate** PR review comment (human or Bugbot) was applied, or the user corrected a pipeline decision.

1. Distill **one reusable Do/Don't** — not the incident, not file names, no PII.
2. If [lessons.md](lessons.md) already covers it, add the source (`PR #n`) and stop.
3. Else append the next `L-{AREA}-{n}` id using the areas in [context.md](context.md).
4. If it is now a durable convention, also add a **one-liner** to the matching standards file in [context.md](context.md) and mark the lesson `promoted`.
5. **Do not harvest** dismissed, invalid, or out-of-scope comments.

Skip harvest on trivial typo-only nits that will never recur as a decision.
