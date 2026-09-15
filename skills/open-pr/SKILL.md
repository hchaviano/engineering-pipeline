---
name: open-pr
description: Turn a finished, verified diff into branch(es) and PR(s). Detects multi-layer diffs and creates stacked PRs, or offers logical splits. Use after verify passes, or whenever a completed diff needs to become a PR.
---

# Open PR

Split an **already-implemented and verified** diff into branch(es) and PR(s). Mirror of `layer-split` (that one splits *before* coding).

Never invoke on a diff that hasn’t been through Self Review + Quality Gate (acceptance is part of that `verify` pass) — those stages are what make the diff “finished”.

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask.

## Execution

Skills cannot pin the chat model. **Parent agents** must run this via `.cursor/agents/open-pr.md`. If you **are** already that agent, ship per the steps below — do not re-launch yourself.

When `/open-pr` is invoked or pipeline Ship starts (parent only):
1. Launch Task with the `open-pr` agent.
2. Pass: ticket, type, description, base, SPEC/REVIEW paths if any, split intent.
3. Do not launch `verify`. If the quality gate has not passed, stop and return that to the parent.

## 1. Collect inputs

Ask in one grouped question if not already known (allowed values: [context.md](context.md)):

- **Ticket number** — mandatory
- **Type** — infer from the diff, confirm
- **Short kebab-case description** — propose from the diff, confirm
- **Base branch** — default in context; confirm if a release line is the target. Resolve from `origin/<base>`, never from another checkout’s dirty state.

## 2. Source of truth

- **Uncommitted:** this worktree is the source. `git checkout -b` carries changes forward. Never `git stash` (shared `refs/stash` — `worktree` skill).
- **Committed on one local branch:** extract subsets with `git checkout <source> -- <files>`; offer to delete source only if never pushed.
- Before any checkout: `git worktree list`. If another worktree holds the branch, stop.

## 3. Detect split

Reuse `layer-split`’s taxonomy and substantiality bar ([context.md](context.md) there).

- **2+ substantial layers** → §4 multi-branch
- **1 layer** → ask whether to split into smaller logical parts; if yes, agree file-group parts with the user, then treat each as a “layer” in §4

## 4. Create stacked branches

For each part, dependency order, using the naming pattern in [context.md](context.md):

1. Name the branch (omit the part suffix if single)
2. Stack off base, then previous
3. Commit only that part’s files
4. Regenerate codegen if context says the part needs it
5. Format only this part’s files (format command in `verify` context) — do **not** launch the `verify` agent. Failure that doesn’t reproduce on base → fix. Failure that does → note pre-existing in the PR body, proceed.
6. Conventional commit message(s) per context

Don’t start the next part until the current one is formatted and committed.

## 5. One confirmation, then push

Ask once, then on yes push and `gh pr create` per [context.md](context.md). Ready for review (never draft) — work is already finished. Unmerged *bases* in a pre-code `layer-split` were draft; here the work is done.

On **single or last** PR: ACs, AC→test matrix, coverage %, self-review verdict.

## 6. Close out

After every PR exists: delete `.cursor/artifacts/SPEC-{task}.md` and `REVIEW-{task}.md`. Offer to delete a superseded local-only source branch.
