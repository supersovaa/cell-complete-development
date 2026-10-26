# cell-complete-development

Skills for implementing systems around concrete domain cells: independently identifiable entities that act as completion and verification boundaries while their internal implementation can be decomposed more finely.

## Skills

- [cell-planning](skills/cell-planning/SKILL.md): plan one cell at a time, decomposing it into implementation plans, recording intra-cell direct dependencies, and persisting cell membership with an explicit unassigned wave.
- [cross-planning](skills/cross-planning/SKILL.md): apply shared diagonal progress planning to relationships between planned Cells, keeping Cell membership and wave handoff explicit.
- [wave-planning](skills/wave-planning/SKILL.md): arrange eligible cell plans using known dependencies and plan-level concurrency analysis, comparing integration effort when a concrete shared change could make safe parallel work costly.
- [wave-planning-review](skills/wave-planning-review/SKILL.md): diagnose avoidable serialization or costly parallel integration from planned boundaries and observed integration work, and identify the responsible planning or design layer.
- [cell-complete-implementation](skills/implementation/SKILL.md): execute one fixed wave while preserving plan boundaries, established concurrency constraints, temporary-test rules, and cell completion conditions.

General plan structure, dependency semantics, individual plan execution, and implementation review remain the responsibility of the corresponding plan-driven skills. `wave-planning-review` uses observed integration work to assess future placements and actionable remaining choices.
Required test cases, expected outcomes, and other test-evidence planning decisions belong to `test-evidence-planning`; wave planning only requires that planning to be complete before fixing a wave.

## Skill setup

Install the skill directories you need from [`skills/`](skills/) into the location used by your agent or skill loader.
For loaders that use one skill per directory, install `skills/cell-planning/`, `skills/cross-planning/`, `skills/wave-planning/`, `skills/wave-planning-review/`, and `skills/implementation/` as separate directories, preserving each directory's `SKILL.md`.

These skills extend the [plan-driven skills](https://github.com/supersovaa/plan-driven-implementation/tree/main/skills): use `plan-driven-planning` with cell, cross, and wave planning, `plan-driven-implementation` with wave execution, and `plan-driven-review` for plan-based review.
Install [diagonal-progress-planning](https://github.com/supersovaa/diagonal-progress-planning/blob/main/SKILL.md) alongside `cross-planning` when coordinating multiple Cells. It owns the general cross-target dependencies, coarse diagonal order, projections, and affected-progress revision.
When fixing a wave for implementation, also make the external [`test-evidence-planning`](https://github.com/supersovaa/requirement-driven-testing/blob/main/skills/planning/SKILL.md) skill available.
Completion of its required planning results is a condition for **fixing a wave**, not for **starting wave planning**.

See [DESIGN.md](DESIGN.md) for the design rationale and shared terminology.
