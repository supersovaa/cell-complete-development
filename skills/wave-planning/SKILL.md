---
name: wave-planning
description: Arrange cross-planned cell plans into current and future waves by completing plan-level concurrency analysis, then require settled test-evidence planning before fixing a wave for implementation.
---

# Wave Planning

Use this skill after cross planning has established cross-cell result dependencies and a coarse implementation order, and the resulting plans are ready for plan-level concurrency analysis and wave assignment.

Use this skill together with the repository's ordinary plan-driven planning rules.
Those rules own dependency semantics, dependency recording, plan state, plan-readiness semantics, durable state, and general concurrency-conflict recording.
This skill owns plan-level concurrency analysis and repository-wide wave arrangement for cell-complete work.

## Complete plan-level concurrency analysis

Read cell membership, cross-cell dependencies, coarse implementation order, and wave assignment from their authoritative records.
Inspect wave-unassigned and already future-assigned cell plans together with the repository-wide plan graph.

Before assigning a plan to a wave, finalize its concurrency constraints using the ordinary planning workflow, including conflicts involving shared implementation.
Treat the dependencies established by cell and cross planning as scheduling inputs rather than rediscovering the cell relationship structure during wave placement.

If concurrency analysis exposes an unresolved requirement or shows that an upstream planning result is no longer valid, stop wave arrangement for the affected work and return the issue to its owning planning workflow before assigning it to a wave.

## Arrange current and future waves

Assign supported cell plans to synchronized waves and persist each assignment in the same authoritative record used for cell membership.
Plan multiple future waves when known dependencies and conflicts support those placements.
Keep an explicit unassigned value for a cell plan whose global placement is not yet justified.

Record each wave's status as future or fixed using the repository's existing wave convention; when none exists, keep one authoritative status record per wave in the implementation index.
Do not duplicate wave fixedness as mutable per-plan state.
Keep assigned waves after the next execution target in future status.
Fix only the earliest not-yet-completed wave selected for execution.
Future wave assignments may be revised until their wave is fixed for implementation.
Only plans in a fixed wave are eligible for implementation through the cell-complete workflow.

Use staircase-shaped diagonal progress as the default pattern:

- a cell contributes at most one plan to a wave;
- additional cells may enter later waves when at least one of their wave-unassigned plans is eligible for placement;
- no existing cell must complete before another cell enters;
- a cell may skip a wave when none of its eligible plans can safely run in that wave;
- a wave completes before the next wave begins.

Use ordinary plan state and dependency results as inputs to wave construction rather than redefining plan readiness or dependency semantics here.

Among viable placements, prefer lighter cells when the available planning information supports a useful distinction.
Do not prescribe a fixed scoring system or a fixed maximum number of active cells.

## Protect shared parts while arranging waves

Unchanged shared implementation may be reused by multiple cell plans in the same wave.

When one cell plan may change a shared part, no other plan in that wave may read, depend on, or modify that shared part.
Represent that mutual exclusion as a concurrency conflict rather than an artificial dependency.

When contention becomes high, keep safe parallelism by skipping conflicting cells or delaying additional cell introduction.
Do not force every active cell to advance in every wave.

## Require test-evidence planning when fixing a wave

Before fixing a wave for implementation, inspect every cell plan assigned to that wave.
Use each plan's settled completion contract as the bounded input to `test-evidence-planning`, or verify that an applicable settled test-evidence planning result already exists.

Require the resulting test definitions and material testing decisions to be complete, recorded using repository conventions, and linked from each plan before fixing the wave.
Ensure the planning scope includes deferred formal coverage that cell planning assigned to a completing plan when its realization condition is present.
Deferred formal coverage whose realization condition is still absent remains outside the current wave's required test-evidence planning scope.

Do not fix the wave or begin its implementation while required test-evidence planning is incomplete.

If test-evidence planning exposes an unresolved requirement or design ambiguity, return that issue to its owning workflow.
If the gate exposes an invalid plan boundary, dependency change, or concurrency conflict, return that issue to its owning planning workflow before fixing the wave.

This skill owns plan-level concurrency completion, wave assignment, and the pre-execution gate that requires completed `test-evidence-planning` for a fixed wave.
Cell-local decomposition and cell-completion criteria belong to cell planning.
Cross-cell result dependencies and coarse cell implementation order belong to cross planning.
Required test cases and expected outcomes belong to `test-evidence-planning`.
Wave execution belongs to cell-complete implementation.
General plan semantics belong to the ordinary planning workflow.
