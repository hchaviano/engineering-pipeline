<!--
Fill when adopting. Leave no <placeholders>.
debugging traces layers and observability named here — not in SKILL.md.
-->
# Debugging — project context

## Isolate (layer order)

Trace UI → business logic → repository → data (or the order below) by reading the actual code:

`<e.g. Presentation → Bloc/Cubit → Repository → Client>`

## Evidence sources

- Crash / error: `<e.g. Crashlytics, Sentry>`
- RUM / sessions: `<e.g. Datadog>`
- Logs: `<structured logger — never print/debugPrint in committed code>`

## Critical flows (impact check)

`<list — same as engineering-pipeline/context.md>`

## Hand-off

2+ layers or specified-behavior change → `engineering-pipeline` at Specify, not a drive-by patch.
