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
2. Pass a self-contained prompt: touched paths / packages, SPEC path when Acceptance Check is required, any known failures to re-check.
3. Return scopes run + pass/fail (plus AC matrix / coverage % / mutation when requested) to the parent. Summaries only — no full test logs.

## Gates

Run the ordered commands in [context.md](context.md) on **touched paths only** unless that file says otherwise. Do not format/analyze the whole repo blindly (pre-existing drift + worktree concurrency). If a wider run rewrites unrelated files, revert those.

When the prompt includes a SPEC path, collect coverage in the **same** test run (command in [context.md](context.md)) so Acceptance Check does not need a second suite.

If any step fails, fix the underlying issue and re-run the failing step(s). Do not call the change done while any required gate is red. Report which scopes ran.

## Acceptance Check (when the prompt includes a SPEC path)

Same pass as the gates — never a second agent, never on the parent. Checks match `architect` (standalone Accept still exists).

**(a) Matrix** — every AC maps to ≥1 test that *asserts* the Then. An AC without one blocks. Do not edit ACs to match the code.

**(b) Coverage** — coverage command in [context.md](context.md) (and `architect`/`verify` context). Below the bar on new/modified testable logic blocks with uncovered lines. Thin passthrough the Coder declared untestable is exempt if still zero-logic.

**(c) AC mutation** — for each AC on a critical flow ([context.md](context.md) / `architect` context) with an explicit boundary, follow the mutation method in `architect` context. **Always revert.** A mutant that still passes blocks.

Return pass/fail per step, the matrix, coverage %, mutation results.
