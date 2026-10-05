# cell-complete-development

Skills for implementing systems around concrete domain cells: independently identifiable entities that act as completion and verification boundaries while their internal implementation can be decomposed more finely.

## Skills

- [cell-complete-planning](skills/planning/SKILL.md): decompose cells into plan-backed stages, arrange them into diagonal conflict-safe waves, and audit required test-definition completeness when fixing a wave for implementation.
- [cell-complete-implementation](skills/implementation/SKILL.md): execute one fixed wave after its pre-execution test-definition gate while preserving plan boundaries, shared-part exclusivity, temporary-test rules, and cell completion conditions.

General plan structure, dependency semantics, individual plan execution, and plan-based review remain the responsibility of the corresponding plan-driven skills.

See [DESIGN.md](DESIGN.md) for the design rationale and shared terminology.
