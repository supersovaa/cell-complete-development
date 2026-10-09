# cell-complete-development

Skills for implementing systems around concrete domain cells: independently identifiable entities that act as completion and verification boundaries while their internal implementation can be decomposed more finely.

## Skills

- [cell-planning](skills/cell-planning/SKILL.md): plan one cell at a time, decomposing it into implementation plans, recording intra-cell direct dependencies, and persisting cell membership with an explicit unassigned wave.
- [cross-planning](skills/cross-planning/SKILL.md): examine dependencies among multiple planned cells when coordination is needed, and record a revisable coarse order in existing Markdown.
- [wave-planning](skills/wave-planning/SKILL.md): arrange eligible cell plans using known dependencies and plan-level concurrency analysis, without waiting for unrelated cross planning.
- [wave-planning-review](skills/wave-planning-review/SKILL.md): review completed wave arrangements for suspicious serialization and identify whether the cause calls for planning changes or shared-implementation design review.
- [cell-complete-implementation](skills/implementation/SKILL.md): execute one fixed wave while preserving plan boundaries, established concurrency constraints, temporary-test rules, and cell completion conditions.

General plan structure, dependency semantics, individual plan execution, and implementation review remain the responsibility of the corresponding plan-driven skills. `wave-planning-review` adds diagnostic review of the completed wave arrangement.
Required test cases, expected outcomes, and other test-evidence planning decisions belong to `test-evidence-planning`; wave planning only requires that planning to be complete before fixing a wave.

## Skill setup

Install the skill directories you need from [`skills/`](skills/) into the location used by your agent or skill loader.
For loaders that use one skill per directory, install `skills/cell-planning/`, `skills/cross-planning/`, `skills/wave-planning/`, `skills/wave-planning-review/`, and `skills/implementation/` as separate directories, preserving each directory's `SKILL.md`.

These skills extend the [plan-driven skills](https://github.com/supersovaa/plan-driven-implementation/tree/main/skills): use `plan-driven-planning` with cell, cross, and wave planning, `plan-driven-implementation` with wave execution, and `plan-driven-review` for plan-based review.
When fixing a wave for implementation, also make the external [`test-evidence-planning`](https://github.com/supersovaa/requirement-driven-testing/blob/main/skills/planning/SKILL.md) skill available.
Completion of its required planning results is a condition for **fixing a wave**, not for **starting wave planning**.

See [DESIGN.md](DESIGN.md) for the design rationale and shared terminology.
