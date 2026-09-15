<!--
Fill when adopting. Leave no <placeholders>.
verify runs only these commands. Do not put stack-specific CLIs in SKILL.md.
-->
# Verify — project context

## Toolchain

- Pin: `<e.g. .fvmrc + fvm flutter / fvm dart; .nvmrc; mise; or “repo Makefile”>`
- If the pin is missing (un-bootstrapped worktree): stop and tell the user to run `<bootstrap e.g. .cursor/scripts/worktree-new.sh>`. Never fall back to a global SDK.

## Gates (in order, pinned toolchain)

1. Format — `<command>` on touched paths only. Never `<format-all e.g. dart format .>` blindly. If a wider run rewrites unrelated files, revert those.
2. Analyze / typecheck / lint — `<command>` on touched paths or package dirs. CLI required; IDE diagnostics alone are not enough.
3. Test — `<command>`: always the tests of every package/module touched by the diff, plus `<app package rule>`. Skipping the full suite is only acceptable when those pass **and** the change doesn’t touch a shared package (`<shared packages>`); otherwise run it in full. When Acceptance Check is in the prompt, collect coverage in this same run (`<coverage flag or make coverage>`) so a second suite is not needed.

## Shared packages (force full suite)

- `<e.g. api_client, ui_kit>`
