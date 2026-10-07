# cell-complete-development

Skills for implementing systems around concrete domain cells: independently identifiable entities that act as completion and verification boundaries while their internal implementation can be decomposed more finely.

## Skills

- [cell-complete-planning](skills/planning/SKILL.md): plan each cell locally without assigning waves, then perform a separate repository-wide pass that completes cross-cell dependency and conflict analysis and assigns cell plans to current and future waves.
- [cell-complete-implementation](skills/implementation/SKILL.md): execute one planned wave after confirming its pre-execution test-definition gate while preserving plan boundaries, shared-part exclusivity, temporary-test rules, and cell completion conditions.

General plan structure, dependency semantics, individual plan execution, and plan-based review remain the responsibility of the corresponding plan-driven skills.

See [DESIGN.md](DESIGN.md) for the design rationale and shared terminology.
