<!--
Abstract review rubric for the self-review skill.
Adopt: copy this file to the consuming project (default path: REVIEW.md at repo root)
and replace every <placeholder>. self-review applies this file as-is — it must not
invent another rubric.

Full coding standards live in the file named below. This file only tunes severity,
scope, always-flag items, and volume.
-->
# Review instructions

Project context, architecture and full coding standards live in `<standards-file>`. This file tunes severity, scope, and what to always flag. **Write all review comments in `<review-language>`.**

`<one-paragraph product + architecture context — what to enforce on new/changed code, how to treat legacy>`

**Critical flows — apply extra scrutiny:** `<list>`. **Sensitive data that must never leak to logs/analytics/insecure storage:** `<list>`.

## Scope

- Judge new and changed code against the standards in `<standards-file>`.
- Pre-existing problems in code this PR didn't touch are 🟣 Pre-existing at most — don't raise them to Important.
- `<legacy / migration rule, or delete this bullet>`: suggesting a refactor toward the standard in untouched code is 🟡 Nit, never Important — unless the PR *introduces new* legacy debt (see Always check).

## What 🔴 Important means here

Reserve 🔴 Important for findings that break behavior, leak data, or violate the architecture in new code:

- **Correctness bugs**: edge cases, lifecycle / async hazards, leaked subscriptions, swallowed errors.
- **Concurrency / races** on operations that can overlap (search, submit, refresh, debounced input) without the explicit policy named in `<standards-file>`.
- **Security (highest priority)**: secrets/tokens/credentials hardcoded; sensitive data in logs or analytics; insecure storage of secrets; unvalidated deep-link / route params.
- **Layer-boundary violations** in new/changed code, per the dependency direction in `<standards-file>`.
- **Unreported errors**: every caught error in business-logic state management must be reported through the project’s error pipeline before a failure state is emitted.

Everything else (clarity, maintainability, naming, structure suggestions) is 🟡 Nit at most.

## Always check (new / changed code)

Flag these even when a generic review pass would skip them:

- **Tests for new testable logic**. Missing tests for new logic → 🔴 Important. Tests must assert behavior, not internals.
- **Hardcoded user-visible strings** instead of the project’s localization system → 🔴 Important (omit if the project has no l10n).
- `<project always-check — e.g. forbidden libraries, DI style, design-system tokens>` → `<severity>`
- `<project always-check>` → `<severity>`

## Do not report

- Anything static analysis already owns (format, import order, compiler/linter style rules).
- Generated files (`<generated-glob>`).
- Legacy patterns present only in code this PR didn't modify.

## Verification & volume

- Behavior claims need a `file:line` citation in the source, not an inference from a name. If you can't verify it, lower the severity or omit it.
- Cap nits at **5 per review**; if there are more, note "plus N similar items" in the summary instead of posting them inline.
- On re-reviews after the first, post 🔴 Important findings only; suppress new nits.

## Summary

Open the review body with a one-line tally (e.g. `2 important, 3 nits`) and lead with "No blocking issues" when there are none. If the PR touches a critical flow or sensitive data (see above), state explicitly whether any security concern was found.
