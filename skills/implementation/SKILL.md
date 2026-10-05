---
name: cell-complete-implementation
description: Execute one planned cell-complete wave by running its cell-stage plans in parallel where allowed, preserving shared-part exclusivity, temporary-test rules, and cell completion boundaries.
---

# Cell-Complete Implementation

Use this skill to execute one already planned cell-complete wave.

Use the repository's ordinary plan-driven implementation rules for every stage plan in the wave.
Those rules own each plan's fixed execution boundary, result recording, completion audit, and replan-required handling.
This skill adds wave orchestration and cell-specific implementation rules.

## Execute one wave

Treat the selected wave as the execution boundary for this orchestration step.

Execute the wave's stage plans in parallel where their recorded constraints allow it.
Do not start work from a later wave before every plan attempt in the current wave has finished its current attempt.

A cell advances by at most one stage in the wave.

If a stage plan cannot proceed because its boundary becomes invalid, follow the ordinary plan-driven stop and replanning workflow.
Do not silently pull a later stage of the same cell into the current wave.

## Preserve completed-cell composition in provisional cells

When a stage provisionally places an incomplete cell, include only components that also belong to the completed cell, with the same kind and role.
Omit components that are unnecessary for the current stage.
Advance the cell by adding components rather than introducing provisional-only components or temporary substitutes that must later be replaced.

## Preserve shared-part exclusivity

Multiple plans may reuse unchanged shared implementation concurrently.

When a plan changes a shared part, no other plan in the same wave may read, depend on, or modify that shared part.
Honor the planned concurrency conflict instead of resolving the collision by concurrent editing.

Shared implementation remains subordinate to cell progress.
Do not expand the current work into speculative shared behavior for future cells.

A shared part created while one cell is incomplete may be reused by another cell.
The originating cell does not need to complete first.

## Use temporary tests for incomplete cells

While a cell is incomplete, use identifiable temporary tests when direct verification of partial work is useful.

Temporary tests may directly verify:

- cell-specific internal parts;
- shared parts;
- partial combinations needed by the current stage.

Use the repository's convention for marking temporary tests.

Do not treat temporary tests as the formal completion guarantee for a cell.

## Complete cells through their final stage

When a stage plan completes its cell, satisfy the cell-completion criteria recorded by planning.

Formal tests must exercise observable required behavior using real repository cells, including the cell being completed, rather than fictional or test-only cells.
A cell does not need a dedicated one-to-one formal test if the formal suite as a whole covers its required behavior.
When completing this cell makes behavior involving other cells available, this later-implemented cell owns the resulting formal coverage.

Remove temporary tests whose responsibility has been transferred to formal completed-cell coverage.
Keep temporary tests that are still needed for other incomplete-cell work.

Run the full automated test suite as required by the final stage plan.
A cell is not complete when that suite reveals a regression in already completed cells.

## Finish the wave

Finish every stage-plan attempt in the wave before advancing to the next wave.

After the wave, return changed planning facts, conflicts, or invalidated future assumptions to the planning workflow before executing a later wave.

This skill owns execution of one cell-complete wave.
Individual plan execution semantics belong to the ordinary plan-driven implementation workflow.
Review remains plan-based and uses the ordinary review workflow.
