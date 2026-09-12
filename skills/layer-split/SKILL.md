---
name: layer-split
description: Split an approved feature/bugfix into one stacked branch + PR per architecture layer. Use when 2+ layers have substantial work, or when the user asks to split into multiple PRs.
---

# Layer Split

Split **before** implementation: create stacked branches first; implement, test, and verify each layer on its own branch. For splitting an already-written diff, use `open-pr`.

**Worktree model:** parallelism is per **feature**, not per layer — all stacked layer branches live in the same worktree (`worktree` skill). Never one worktree per layer.

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask.

## Execution

Skills cannot pin the chat model. **Parent agents** must run this via `.cursor/agents/layer-split.md`. If you **are** already that agent, execute the split below — do not re-launch yourself.

When `/layer-split` is invoked or the pipeline requests a split (parent only):
1. Launch Task with the `layer-split` agent.
2. Pass: task number, type, description, base branch, layer map if known.
3. Layer verify steps use the `verify` agent.

## When to split

Only when **2 or more layers** have substantial work (new client/module, repository with real logic, new state container, multi-screen UI — examples in [context.md](context.md)). Small features stay a single branch/PR — **never force the split**.

Possible layers, in order, used only as applicable: the identifiers in [context.md](context.md).

## Inputs (one grouped question)

- **Task number** — mandatory. Always ask if it wasn’t given.
- Type: values in [context.md](context.md)
- Short kebab-case description — propose one, confirm it
- Base branch — default in [context.md](context.md); if an active release branch is the target, confirm which

## Branch naming / stacking / PRs / gates / commits

Follow the patterns in [context.md](context.md). Show the full table (branches, bases, per-layer content) and **wait** before creating branches. Explicit confirmation before `git push` / `gh pr create`. Before each layer commit: launch `verify`; that layer’s new/modified testable logic has its test in the same PR.

## Backward propagation

Fixes for layer N−1 go on N−1’s branch — never patch from N — then rebase the chain forward (`git rebase --update-refs` when available). Check `git worktree list` first — cannot rebase a branch checked out in another worktree.
