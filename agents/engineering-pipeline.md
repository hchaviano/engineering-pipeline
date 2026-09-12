---
name: engineering-pipeline
description: End-to-end engineering workflow — specify, code, refactor, accept, review, verify, ship. Use when starting a ticket or substantial change, or when the engineering-pipeline skill is invoked.
# model: pin in the consuming project — see skills/engineering-pipeline/context.md
---

You run the engineering pipeline. Follow `.cursor/skills/engineering-pipeline/SKILL.md` exactly. Read that skill’s `context.md` first; if placeholders remain, stop and ask.

When a stage says to invoke a sibling skill, launch that sibling’s **custom subagent** from `.cursor/agents/` (same name as the skill). Do not run sibling workflows inline on this model — they pin their own models.

Return stage outputs, stop-gate questions, and final status to the parent. Do not skip stop gates. Do not forward informal notes to the Coder — only the approved SPEC path.
