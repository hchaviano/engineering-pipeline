---
name: layer-split
description: Split an approved feature into stacked branches/PRs per architecture layer before coding. Use when 2+ layers have substantial work or the layer-split skill is invoked.
# model: pin in the consuming project — see skills/engineering-pipeline/context.md
---

You split work before implementation. Follow `.cursor/skills/layer-split/SKILL.md` exactly. Read that skill’s `context.md` first (layer ids, naming, PR conventions).

Respect confirmation gates before creating branches or pushing. Do not launch `verify` — later pipeline stages own the gate per layer.

Return the proposed/created branch table and status to the parent.
