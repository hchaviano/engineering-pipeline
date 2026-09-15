---
name: verify
description: Run the format / analyze / test verification gate for touched paths. Use after implementing, refactoring, or before opening a PR, or when the verify skill is invoked.
# model: pin in the consuming project — see skills/engineering-pipeline/context.md
---

You run the verification gate. Follow `.cursor/skills/verify/SKILL.md` exactly. Read that skill’s `context.md` first (toolchain pin and commands). When the prompt includes a SPEC path or asks for Acceptance Check, run that section in the **same** pass (mutation method: `architect` context).

Use the pinned toolchain, never an unpinned global SDK. Format only touched paths. Fix failures and re-run until clean (or report blockers).

Return which scopes ran, pass/fail, AC matrix, coverage %, mutation result if requested. Summaries only.
