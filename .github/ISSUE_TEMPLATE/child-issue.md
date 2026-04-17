---
name: "🗂️ Planning child issue"
about: Concrete work item that belongs to a planning epic (not a bug report — use the bug template for those).
title: "[<epic-slug>] "
labels: planning
assignees: ''
---

## Parent epic
Part of zenpai45/ustepperSTM32.plan#<N>
<!-- Replace <N> with the epic issue number. GitHub renders this as a timeline reference on the epic. -->

## What
<!-- One sentence: what this delivers. -->

## Where
<!-- File(s) / directory this lands in. See sibling cheat sheet in ../ustepperSTM32.plan/CLAUDE.md. -->
-

## Acceptance criteria
- [ ]

## BSP impact
- [ ] None — internal only
- [ ] Touches `variants/` — new or modified pin map / peripheral pins
- [ ] Touches `boards.txt` / `platform.txt` / `package.json` — board menu or upload recipe changed
- [ ] Touches a bundled `libraries/<name>/` — that lib's `library.properties` version bumped
- [ ] Requires new entry in `.github/workflows/ustepper-compile.yml`

## Testing
<!-- Compile against which variant, which example, any on-hardware verification. -->
