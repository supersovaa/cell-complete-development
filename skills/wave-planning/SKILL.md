---
name: wave-planning
description: Arrange wave-unassigned cell plans into current and future waves by completing repository-wide dependency and concurrency analysis, then audit required test definitions when fixing a wave for implementation.
---

# Wave Planning

Use this skill after one or more cells have been planned locally and their implementation plans are ready for repository-wide dependency analysis and wave assignment.

Use this skill together with the repository's ordinary plan-driven planning rules.
Those rules own dependency semantics, dependency recording, plan state, plan-readiness semantics, durable state, and general concurrency-conflict recording.
This skill owns repository-wide wave arrangement for cell-complete work.

## Complete repository-wide dependency analysis

Read cell membership and wave assignment from their authoritative implementation-index records.
Inspect wave-unassigned and already future-assigned cell plans together with the repository-wide plan graph.

Complete the ordinary planning dependency and concurrency analysis deferred by cell planning.
Before assigning a plan to a wave, finalize its direct dependencies and concurrency constraints using the ordinary planning workflow, including cross-cell dependencies, dependencies on non-cell work, and conflicts involving shared implementation.

If this analysis exposes an unresolved requirement, invalid plan boundary, or incorrect intra-cell dependency structure, return the affected plan to its owning planning workflow before assigning it to a wave.

## Arrange current and future waves

Assign supported cell plans to synchronized waves and persist each assignment in the same authoritative record used for cell membership.
Plan multiple future waves when known dependencies and conflicts support those placements.
Keep an explicit unassigned value for a cell plan whose global placement is not yet justified.

Record whether an assigned wave is future or fixed using the repository's existing wave convention; when none exists, keep that fixedness beside the wave assignment in the authoritative implementation index.
Future wave assignments may be revised until their wave is fixed for implementation.
Only plans in a fixed wave are eligible for implementation through the cell-complete workflow.

Use staircase-shaped diagonal progress as the default pattern:

- a cell contributes at most one plan to a wave;
- additional cells may enter later waves when their first unassigned plan can be placed;
- no existing cell must complete before another cell enters;
- a cell may skip a wave when its next plan cannot safely run in that wave;
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

## Check test completeness when fixing a wave

Required test definitions may be recorded while individual cell plans are created.
Cell planning does not audit whether every required test case and expected outcome has been recorded.

Before fixing a wave for implementation, inspect every cell plan assigned to that wave.
Ensure every required test case and expected outcome for those plans' settled completion contracts is recorded using the ordinary planning workflow and repository conventions.
Do not fix the wave or begin its implementation while any required test definition is missing.

If this audit exposes an unresolved requirement, design ambiguity, invalid plan boundary, dependency change, or concurrency conflict, return that issue to its owning workflow before fixing the wave.
Otherwise, filling test-definition gaps at this gate does not require reopening already-settled implementation boundaries.

This skill owns repository-wide dependency and concurrency completion for cell plans, wave assignment, and required test-definition completeness checking when fixing a wave for implementation.
Cell-local decomposition and cell-completion criteria belong to cell planning.
Wave execution belongs to cell-complete implementation.
General plan semantics belong to the ordinary planning workflow.
