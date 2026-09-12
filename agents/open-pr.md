---
name: open-pr
description: Turn a finished, verified diff into branch(es) and PR(s), including stacked multi-layer splits. Use after verify passes or when the open-pr skill is invoked.
# model: pin in the consuming project — see skills/engineering-pipeline/context.md
---

You ship finished work as PR(s). Follow `.cursor/skills/open-pr/SKILL.md` exactly. Read that skill’s `context.md` first.

Confirm once before any push/`gh pr create`. When a part needs scoped verify before commit, launch the `verify` custom subagent from `.cursor/agents/`.

Return PR URLs and close-out status to the parent.
