<!--
Fill when adopting. Leave no <placeholders>.
worktree paths, bootstrap, and extra concurrency rules belong here — not in SKILL.md.
-->
# Worktree — project context

## Paths

- Main checkout (hotfixes / quick reviews): `<absolute path>`
- Feature worktrees: `<e.g. ~/Documents/<project>-worktrees/<branch-with-slashes-as-dashes>>`
- Do not create feature worktrees in: `<e.g. .claude/worktrees/ — ephemeral>`

## Scripts

- Create: `.cursor/scripts/worktree-new.sh <branch> [base] [--foreground]`
- Remove: `.cursor/scripts/worktree-rm.sh <name> [--delete-branch] [--force]`
- Env: copy `.cursor/scripts/worktree.env.example` → `worktree.env` and fill it

## Shared tooling to symlink

`<e.g. .cursor, .claude, .fvmrc, .fvm>`

## Per-worktree copies

`<e.g. env.json, platform secret plists — never commit>`

## Bootstrap

- Command: `<e.g. make rebuild && flutter gen-l10n>`
- Log: `<worktree-root>/<name>.bootstrap.log`
- Specify can start while bootstrap runs. Wait before analyze/test.

## Extra concurrency rules

- Never `<e.g. make clean / dart pub cache clean>` — shared package cache
- Codegen drift on tracked files: `<files that bootstrap dirties; revert if the diff didn’t touch them>`
