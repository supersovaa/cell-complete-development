# cell-complete-development

Skills for implementing systems around concrete domain cells: independently identifiable entities that act as completion and verification boundaries while their internal implementation can be decomposed more finely.

## Skills

- [cell-planning](skills/cell-planning/SKILL.md): plan one cell at a time, decomposing it into implementation plans and recording only intra-cell direct dependencies while leaving wave assignment unset.
- [wave-planning](skills/wave-planning/SKILL.md): complete repository-wide dependency and concurrency analysis, assign supported cell plans to current and future waves, and audit required test-definition completeness when fixing a wave.
- [cell-complete-implementation](skills/implementation/SKILL.md): execute one fixed wave while preserving plan boundaries, shared-part exclusivity, temporary-test rules, and cell completion conditions.

General plan structure, dependency semantics, individual plan execution, and plan-based review remain the responsibility of the corresponding plan-driven skills.

See [DESIGN.md](DESIGN.md) for the design rationale and shared terminology.
