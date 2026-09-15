<!--
Per-task spec for engineering-pipeline.
Lifecycle: draft (specifier) → approved (plan stop gate, frozen) → implemented (coder) → verified (architect + verify)
On verified: export ACs + Traceability + coverage % + self-review verdict to the PR body, then delete
(with REVIEW-{task}.md) after PRs exist. With layer-split, defer deletion to the chain's last PR.

Fill extra sections required by skills/specifier/context.md (UI states, l10n, analytics, …).
-->
---
task: <task number>
title: <short title>
status: draft
created: <date>
---

## Context
<!-- 2-3 lines; link to the ticket -->

## Decisions
<!-- Challenge stage: objection → decision. Architectural items become ADRs at close if context says so. -->

## Acceptance Criteria
<!-- AC-1: Given <state> When <action> Then <observable result>. Light G/W/T, numbered for traceability. -->

## UI States
<!-- loading / success / empty / failure, per screen — omit if not a UI change -->

## Errors
<!-- expected error → user-facing state/message (non-technical) -->

## Localization
<!-- new l10n keys — omit if context says the project has no l10n -->

## Analytics
<!-- events, if applicable -->

## Traceability
<!-- Filled by the coder. AC-1 → test/path/to/file::group name -->

## Out of scope
<!-- explicit; keeps the coder from filling gaps from informal context it never received -->

## Implementation Phases
<!-- One phase per context window. Resume after a new chat at the first phase whose Status is not `done`. -->

### Phase 1 — <name>
- **Status:** todo
- **Scope:** <one sentence>
- **Files:** <paths>
- **ACs:** AC-1
