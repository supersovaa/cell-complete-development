---
name: cross-planning
description: Coordinate partial results and dependencies across planned Cells using diagonal progress planning. Leave wave assignment to wave planning.
---

# Cross Planning

For coordination across planned Cells, start with the partial results from `cell-planning`. Apply `diagonal-progress-planning` to those results and `plan-driven-planning` for dependency semantics and authoritative planning records.

## Apply Cell boundaries

Keep every Cell plan's authoritative membership in exactly one Cell.
A partial result in one Cell may serve another Cell before its owning Cell completes.
Apply ordinary planning's distinction between pending producing plans and results already established on the relevant base branch. Shared implementation remains subordinate to the Cell plan that establishes it.

Record confirmed cross-Cell direct dependencies in the existing implementation index.
Keep the coarse, revisable order of Cells in an appropriate existing Markdown planning document, distinct from direct dependencies.
Return needed Cell-local plan splitting and completion-criteria corrections to `cell-planning`.

## Hand off to wave planning

Provide the confirmed dependencies and any useful coarse order to `wave-planning`.
Wave planning owns plan-level concurrency analysis, wave assignment, and the test-evidence gate before a wave is fixed.
Independent eligible Cell plans remain available for wave arrangement while unrelated cross-Cell planning continues.

This skill owns the Cell-specific interpretation and recording of cross-target planning.
The shared `diagonal-progress-planning` skill owns cross-target ordering, staged progress, projection, and affected-progress revision.
