---
name: debugging
description: Root-cause-first debugging — reproduce, isolate, validate a hypothesis with evidence, then propose the minimal fix plus a regression test. Use when the user reports a bug, crash, or unexpected behavior, BEFORE modifying any code.
---

# Debugging

Don’t edit code before working through this.

**Project context:** read [context.md](context.md) before executing. If `<placeholders>` remain, stop and ask.

## Execution

Skills cannot pin the chat model. **Parent agents** must run this via `.cursor/agents/debugging.md`. If you **are** already that agent, execute the process below — do not re-launch yourself.

When `/debugging` is invoked or a bug is reported (parent only):
1. Launch Task with the `debugging` agent.
2. Pass: symptoms, repro steps, environment, logs/links.
3. Hand-offs to `engineering-pipeline` or `verify` use those agents in `.cursor/agents/`.

## Process

1. **Reproduce** — formalize steps (version, environment, user state). If unreproducible, say so and work from evidence (crash reporter, RUM, logs — sources in [context.md](context.md)).

2. **Isolate** — trace through the layers in [context.md](context.md) by reading the actual code, never assuming from names.

3. **Hypothesize & validate** — explicit root cause + confirming evidence (failing test, observed value). **No fix proposal without a validated hypothesis.**

4. **Propose** — root cause + minimal fix + impact on neighboring critical flows ([context.md](context.md)).
   **Stop gate: if the user only reported the bug, deliver diagnosis and wait — don’t apply the fix unless asked.**
   If the validated fix spans 2+ layers or changes specified behavior → hand to `engineering-pipeline` at Specify with a minimal spec.

5. **Fix + regression test** — test that fails without the fix and passes with it.

6. **Verify** — launch `verify` **once** (including Acceptance Check if a SPEC exists). Do not run the gate inline. Re-check neighboring critical flows from the verify summary.
