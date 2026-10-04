---
name: cell-complete-planning
description: Plan implementation around concrete domain cells as completion boundaries, decompose each cell into plan-backed stages, and arrange those stages into conflict-safe diagonal waves.
---

# Cell-Complete Planning

Use this skill when a repository defines concrete domain entities as cells and wants implementation to progress across multiple cells without making shared parts independent completion targets.

Use this skill together with the repository's ordinary plan-driven planning rules. Those rules own plan structure, dependencies, readiness, durable state, and general concurrency-conflict recording. This skill adds cell and wave semantics.

## Establish the cell boundary

Use the repository's existing definition of a cell when one exists.
Otherwise establish which concrete, individually identifiable domain entities count as cells before planning cell-complete work.

Treat a cell as a completion and verification boundary, not as a required code-ownership or dependency boundary.

## Decompose cells into stage plans

Decompose each cell into stages that each establish one concrete partial result toward completing that cell.

Represent every stage as exactly one implementation plan.
Each implementation plan belongs to exactly one stage of exactly one cell.

Do not create a cell-independent plan merely to implement a shared effect, helper, mechanism, or reusable abstraction.
A cell-stage plan may create, change, or extract shared implementation when that change is required to establish the stage result.

Keep enough forward structure to guide implementation, but do not freeze the complete future stage sequence for a cell.
Revise later stages as earlier waves establish new facts.
Use the ordinary plan workflow to fix the near-term plan boundary when a stage is selected for implementation.

## Prefer lighter cells

Among cells whose next stage is viable, prefer lighter cells when the available planning information supports a useful distinction.

Do not prescribe a fixed scoring system or a fixed maximum number of active cells.

## Build diagonal waves

Arrange stage plans into synchronized waves.

Use staircase-shaped diagonal progress as the default pattern:

- an active cell advances by at most one stage in a wave;
- additional cells may enter later waves when their first stage becomes viable;
- no existing cell must complete before another cell enters;
- a cell may skip a wave when its next stage cannot safely run in that wave.

A wave is complete before the next wave begins.

Use ordinary plan readiness and dependency results as inputs to wave construction rather than redefining dependency semantics here.

## Protect shared parts

Unchanged shared implementation may be reused by multiple stage plans in the same wave.

When one stage plan may change a shared part, no other plan in that wave may read, depend on, or modify that shared part.
Represent that mutual exclusion as a concurrency conflict rather than an artificial dependency.

When contention becomes high, keep safe parallelism by skipping conflicting cells or delaying additional cell introduction.
Do not force every active cell to advance in every wave.

## Plan cell completion

The final stage plan for a cell must make cell completion explicit.

Its completion criteria must require:

- all behavior required of the cell to be implemented;
- the cell's observable required behavior to be covered by formal tests using real repository cells, including the cell being completed;
- obsolete temporary tests whose role has moved to formal cell-level verification to be removed;
- newly established behavior that depends on combinations of cells to receive formal coverage once every participating cell is either already complete or at its final completion stage; and
- the full automated test suite to pass.

Formal coverage does not require one dedicated test case or test file per cell.

Do not require fictional or test-only cells for formal tests.
Mocks, stubs, fixtures, and similar test doubles remain available for dependencies that are not cells.

This skill owns cell decomposition, cell-stage plan creation, and wave construction.
General plan semantics belong to the ordinary planning workflow.
Wave execution belongs to cell-complete implementation.
