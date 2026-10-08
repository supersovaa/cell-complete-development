---
name: cell-complete-implementation
description: Execute one fixed cell-complete wave by running its cell plans in parallel where allowed, honoring plan-level concurrency constraints, temporary-test rules, and cell completion boundaries.
---

# Cell-Complete Implementation

Use this skill to execute one already fixed cell-complete wave.

Use the repository's ordinary plan-driven implementation rules for every cell plan in the wave.
Those rules own each plan's fixed execution boundary, result recording, completion audit, and replan-required handling.
This skill adds wave orchestration and cell-specific implementation rules.

## Execute one wave

Treat the selected wave as the execution boundary for this orchestration step.
Before execution, confirm that wave planning verified completed `test-evidence-planning` for every plan when fixing this wave for implementation.
If that pre-execution gate is incomplete, return to wave planning instead of beginning implementation.

Execute the wave's cell plans in parallel where their recorded constraints allow it.
Do not start work from a later wave before every plan attempt in the current wave has finished its current attempt.

A cell contributes at most one plan to the wave.

If a cell plan cannot proceed because its boundary becomes invalid, follow the ordinary plan-driven stop and replanning workflow.
Do not substitute another plan from the same cell into the already fixed wave.

## Preserve completed-cell composition in provisional cells

When a cell plan provisionally places an incomplete cell, include only components that also belong to the completed cell, with the same kind and role.
Omit components that are unnecessary for the current plan.
Advance the cell by adding components rather than introducing provisional-only components or temporary substitutes that must later be replaced.

## Honor plan-level shared-part constraints

Multiple plans may use shared implementation concurrently, including while another plan changes it, when their contracts and implementation assumptions remain compatible and their changes can be integrated independently.
Sharing a file or module is not by itself an execution conflict.

Honor the recorded concurrency conflicts and implement each plan within its fixed boundary.
Coordinate independently compatible edits to shared files so integration preserves each plan's required behavior and verification without silently overwriting another plan's work.

If actual work reveals an unrecorded conflict, such as incompatible overlapping edits or an invalidated shared contract or implementation assumption, stop the affected attempt and return the changed conflict or plan boundary to the owning planning workflow before continuing that work.
Follow the ordinary plan-driven rules for recording a stopped or invalidated attempt.

Shared implementation remains subordinate to cell progress.
Do not expand the current work into speculative shared behavior for future cells.

A shared part created while one cell is incomplete may be reused by another cell.
The originating cell does not need to complete first.

## Use temporary tests for incomplete cells

While a cell is incomplete, use identifiable temporary tests when direct verification of partial work is useful.

Temporary tests may directly verify:

- cell-specific internal parts;
- shared parts;
- partial combinations needed by the current cell plan.

Use the repository's convention for marking temporary tests.

Do not treat temporary tests as the formal completion guarantee for a cell.

## Complete cells through their completing plan

When a cell plan completes its cell, satisfy the cell-completion criteria recorded by cell planning.

Formal tests must exercise observable required behavior using real repository cells, including the cell being completed, rather than fictional or test-only cells.
A cell does not need a dedicated one-to-one formal test if the formal suite as a whole covers its required behavior.
When completing this cell makes behavior involving other cells available, this later-implemented cell owns the resulting formal coverage.
When cell planning assigned previously deferred formal coverage to this completing plan because the case is now realizable, establish that formal coverage before completing the cell.
Deferred formal coverage whose realization condition is still absent does not block completion of the current cell.

Remove temporary tests whose responsibility has been transferred to formal completed-cell coverage.
Keep temporary tests that are still needed for other incomplete-cell work.

Run the full automated test suite as required by the completing cell plan.
A cell is not complete when that suite reveals a regression in already completed cells.

## Finish the wave

Finish every cell-plan attempt in the wave before advancing to the next wave.

After the wave, return changed plan boundaries or dependencies to their owning planning workflow and changed wave assumptions to wave planning before executing a later wave.
Wave planning may then revise future assignments that are not already fixed.

This skill owns execution of one cell-complete wave.
Individual plan execution semantics belong to the ordinary plan-driven implementation workflow.
Review remains plan-based and uses the ordinary review workflow.
