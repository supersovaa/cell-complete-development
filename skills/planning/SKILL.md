---
name: cell-complete-planning
description: Plan concrete domain cells in two passes: first create wave-unassigned cell plans using only intra-cell dependencies, then perform global dependency and conflict analysis and assign supported plans to current and future waves.
---

# Cell-Complete Planning

Use this skill when a repository defines concrete domain entities as cells and wants implementation to progress across multiple cells without making shared parts independent completion targets.

Use this skill together with the repository's ordinary plan-driven planning rules.
Those rules own plan structure, dependency semantics, plan-readiness semantics, durable state, and general concurrency-conflict recording.
This skill separates cell-local planning from global wave planning and adds cell and wave semantics.

## Establish the cell boundary

Use the repository's existing definition of a cell when one exists.
Otherwise establish which concrete, individually identifiable domain entities count as cells before planning cell-complete work.

Treat a cell as a completion and verification boundary, not as a required code-ownership or dependency boundary.

## Plan each cell locally

Plan one cell at a time.

Decompose the cell into implementation plans that each establish one concrete partial result toward completing that cell.
Each such plan belongs to exactly one cell.

During this pass, inspect and record only prerequisite relations between plans of the same cell.
Do not treat the ordinary repository-wide direct-dependency set as complete yet.
Do not perform repository-wide dependency analysis, compare the cell with other cells, or assign its plans to waves.

Leave every plan produced by this pass wave-unassigned.
Wave assignment is separate from durable plan state; wave-unassigned does not add a new plan state.

When a plan provisionally places an incomplete cell, plan it as a reduced form of the completed cell.
Every component included in the provisional cell must also belong to the completed cell with the same kind and role.
Omit components that are unnecessary at that point, and plan later work to extend the cell by adding components rather than replacing provisional-only components or temporary substitutes.

Do not create a cell-independent plan merely to implement a shared effect, helper, mechanism, or reusable abstraction.
A cell plan may create, change, or extract shared implementation when that change is required to establish the plan result.

Keep enough future cell-local structure to guide implementation, but do not invent plan boundaries that available facts do not yet support.

## Build waves globally

After cell-local planning, inspect wave-unassigned and already future-assigned cell plans together with the repository-wide plan graph.

Complete the ordinary planning dependency and concurrency analysis deferred by cell-local planning.
Before assigning a plan to a wave, finalize its direct dependencies and concurrency constraints using the ordinary planning workflow, including cross-cell dependencies, dependencies on non-cell work, and conflicts involving shared implementation.

If this global analysis exposes an unresolved requirement, invalid plan boundary, or incorrect cell-local dependency structure, return the affected plan to the ordinary planning workflow before assigning it to a wave.

Assign supported cell plans to synchronized waves.
Plan multiple future waves when known dependencies and conflicts support those placements.
Leave a cell plan wave-unassigned when its global placement is not yet justified.

Future wave assignments may be revised until their wave is fixed for implementation.

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
Cell-local planning does not audit whether every required test case and expected outcome has been recorded.

Before fixing a wave for implementation, inspect every cell plan assigned to that wave.
Ensure every required test case and expected outcome for those plans' settled completion contracts is recorded using the ordinary planning workflow and repository conventions.
Do not fix the wave or begin its implementation while any required test definition is missing.

If this audit exposes an unresolved requirement, design ambiguity, invalid plan boundary, dependency change, or concurrency conflict, return that issue to its owning workflow before fixing the wave.
Otherwise, filling test-definition gaps at this gate does not require reopening already-settled implementation boundaries.

## Plan cell completion

The cell plan that completes a cell must make cell completion explicit.

Its completion criteria must require:

- all behavior required of the cell to be implemented;
- the cell's observable required behavior to be covered by formal tests using real repository cells, including the cell being completed;
- obsolete temporary tests whose role has moved to formal cell-level verification to be removed;
- newly established behavior that depends on combinations of cells to receive formal coverage in the completing plan of the cell implemented later, when that cell makes the combined behavior available; and
- the full automated test suite to pass.

Formal coverage does not require one dedicated test case or test file per cell.

Do not require fictional or test-only cells for formal tests.
Mocks, stubs, fixtures, and similar test doubles remain available for dependencies that are not cells.

This skill owns cell-local decomposition, the wave-unassigned state between planning passes, global wave construction, and required test-definition completeness checking when fixing a wave for implementation.
General plan semantics belong to the ordinary planning workflow.
Wave execution belongs to cell-complete implementation.
