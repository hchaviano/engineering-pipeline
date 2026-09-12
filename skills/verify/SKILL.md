---
name: verify
description: Run the format / analyze / test verification gate before considering any change done. Use after implementing, after a refactor, or before opening a PR.
---

# Verify

**Precondition:** the toolchain pin in [context.md](context.md) must be usable from the working directory. If it isn’t, **stop** and tell the user how to bootstrap (that file). Never fall back to an unpinned global toolchain.

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask.

## Execution

Skills cannot pin the chat model. **Parent agents** must run this via `.cursor/agents/verify.md`. If you **are** already that agent, run the gates below — do not re-launch yourself.

When `/verify` is invoked or a sibling asks for the gate (parent only):
1. Launch Task with the `verify` agent.
2. Pass a self-contained prompt: touched paths / packages, any known failures to re-check.
3. Return scopes run + pass/fail to the parent.

## Gates

Run the ordered commands in [context.md](context.md) on **touched paths only** unless that file says otherwise. Do not format/analyze the whole repo blindly (pre-existing drift + worktree concurrency). If a wider run rewrites unrelated files, revert those.

If any step fails, fix the underlying issue and re-run the failing step(s). Do not call the change done while any required gate is red. Report which scopes ran.
