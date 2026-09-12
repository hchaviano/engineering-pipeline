---
name: worktree
description: Create, list, and remove parallel feature worktrees, and follow concurrency guardrails. Use when starting a feature while another is in progress, when asked to work in parallel, or before git operations that could collide with a sibling worktree.
---

# Worktree

One worktree per feature, one Cursor session per worktree. Parallelism is **across features** — a feature’s stacked layer branches all live inside that feature’s worktree (`layer-split`).

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask.

## Create

Run the create script named in [context.md](context.md). It should: create the worktree off `origin/<base>`, symlink shared personal tooling, copy per-worktree secrets/config, and run the bootstrap command.

While bootstrap runs you can already do Specify (Understand / Challenge / Spec). Wait for it before analyze/test.

Then open the worktree in a new Cursor session → `/engineering-pipeline`.

## Inspect / remove

- `git worktree list`
- The remove script in [context.md](context.md) — after the feature’s PRs are merged. Refuses on uncommitted changes unless `--force`.

## Concurrency guardrails (every session)

Worktrees share one git object store, one `refs/` namespace, and usually one package cache. Follow every rule in [context.md](context.md). At minimum:

1. **Never `git stash`.** `refs/stash` is repo-wide. Instead: `git commit -m "wip" --no-verify` and `git reset --soft HEAD~1` when resuming.
2. **A branch can only be checked out in one worktree.** Before checkout/`branch -f`/rebase of a branch you didn’t create here, check `git worktree list`. If another worktree holds it, stop.
3. **Format only files of your own diff** — never format the whole tree.
4. Shared `.cursor/` is intentional. Artifact names are per-task (`SPEC-{task}.md`) — keep slugs unique across concurrent features.
