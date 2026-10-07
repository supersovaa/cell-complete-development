# cell-complete-development

Skills for implementing systems around concrete domain cells: independently identifiable entities that act as completion and verification boundaries while their internal implementation can be decomposed more finely.

## Skills

- [cell-planning](skills/cell-planning/SKILL.md): plan one cell at a time, decomposing it into implementation plans, recording intra-cell direct dependencies, and persisting cell membership with an explicit unassigned wave.
- [wave-planning](skills/wave-planning/SKILL.md): consume those persisted cell plans, complete repository-wide dependency and concurrency analysis, assign supported plans to current and future waves, and require completed test-evidence planning when fixing a wave.
- [cell-complete-implementation](skills/implementation/SKILL.md): execute one fixed wave while preserving plan boundaries, shared-part exclusivity, temporary-test rules, and cell completion conditions.

General plan structure, dependency semantics, individual plan execution, and plan-based review remain the responsibility of the corresponding plan-driven skills.
Required test cases, expected outcomes, and other test-evidence planning decisions belong to `test-evidence-planning`; wave planning only requires that planning to be complete before fixing a wave.

See [DESIGN.md](DESIGN.md) for the design rationale and shared terminology.
