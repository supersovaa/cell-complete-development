# cell-complete-development

Skills for implementing systems around concrete domain cells: independently identifiable entities that act as completion and verification boundaries while their internal implementation can be decomposed more finely.

## Skills

- [cell-planning](skills/cell-planning/SKILL.md): plan one cell at a time, decomposing it into implementation plans, recording intra-cell direct dependencies, and persisting cell membership with an explicit unassigned wave.
- [wave-planning](skills/wave-planning/SKILL.md): consume cross-planned cell plans, complete plan-level concurrency analysis, assign supported plans to current and future waves, and require completed test-evidence planning when fixing a wave.
- [wave-planning-review](skills/wave-planning-review/SKILL.md): review completed wave arrangements for suspicious serialization and direct the user to the planning layer that should be reconsidered.
- [cell-complete-implementation](skills/implementation/SKILL.md): execute one fixed wave while preserving plan boundaries, shared-part exclusivity, temporary-test rules, and cell completion conditions.

General plan structure, dependency semantics, individual plan execution, and plan-based review remain the responsibility of the corresponding plan-driven skills.
Required test cases, expected outcomes, and other test-evidence planning decisions belong to `test-evidence-planning`; wave planning only requires that planning to be complete before fixing a wave.

## Skill setup

Install the skill directories you need from [`skills/`](skills/) into the location used by your agent or skill loader.
For loaders that use one skill per directory, install `skills/cell-planning/`, `skills/wave-planning/`, `skills/wave-planning-review/`, and `skills/implementation/` as separate directories, preserving each directory's `SKILL.md`.

These skills extend the [plan-driven skills](https://github.com/supersovaa/plan-driven-implementation/tree/main/skills): use `plan-driven-planning` with cell and wave planning, `plan-driven-implementation` with wave execution, and `plan-driven-review` for plan-based review.
When fixing a wave for implementation, also make the external [`test-evidence-planning`](https://github.com/supersovaa/requirement-driven-testing/blob/main/skills/planning/SKILL.md) skill available.
Completion of its required planning results is a condition for **fixing a wave**, not for **starting wave planning**.

See [DESIGN.md](DESIGN.md) for the design rationale and shared terminology.
