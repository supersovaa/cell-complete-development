# Cell-Complete Development Design

## Purpose

Cell-complete development treats a concrete domain entity as the minimum completion boundary while allowing implementation work to be decomposed more finely and to progress diagonally across multiple cells.

It is intended for domains that naturally contain moderately sized, clearly separable entities such as individual cards or characters.

## Cell

A cell is a concrete, individually identifiable domain entity whose completion can be judged independently.

The exact entity type that constitutes a cell is repository-specific.
Use an existing repository definition when available; otherwise establish the cell boundary before applying this method.

A cell is a completion and verification boundary, not necessarily a code-ownership, dependency, or implementation boundary.

## Two-pass planning

Cell-complete planning separates cell-local decomposition from repository-wide wave arrangement.

### Cell-local planning

Plan one cell at a time.

Decompose the cell into implementation plans that each establish one concrete partial result toward completing that cell.
One such plan belongs to exactly one cell.

During this pass, inspect and record only dependencies between plans of the same cell.
Do not perform repository-wide dependency analysis, compare the cell with other cells, or assign its plans to waves.

A plan produced by this pass is wave-unassigned until global wave planning places it.

Do not create an independent plan merely to implement a shared effect, helper, mechanism, or reusable abstraction.
A cell plan may create, change, or extract shared implementation when that work is required to establish the cell plan's result.

Keep enough future cell-local structure to guide later work, but do not invent plans whose boundaries cannot yet be established from available facts.

### Global wave planning

After cell-local planning, inspect wave-unassigned and already future-assigned cell plans together with the repository-wide plan graph.

Complete the dependency analysis that cell-local planning intentionally deferred.
This includes cross-cell dependencies, dependencies on non-cell work, and concurrency conflicts involving shared implementation.

If this global analysis shows that a cell plan's boundary or cell-local dependency structure is invalid, return that plan to the ordinary planning workflow before assigning it.

Assign supported cell plans to synchronized waves.
Plan multiple future waves when the known dependencies and conflicts are sufficient to do so.
A cell plan may remain wave-unassigned when its global placement is not yet justified.

Future wave assignments remain revisable until their wave is fixed for implementation.

A typical progression is:

- wave 1: A1;
- wave 2: A2 and B1;
- wave 3: A3, B2, and C1.

Here A, B, and C are cells, and each numbered item is a cell plan.

Use staircase-shaped diagonal progress as the default:

- a cell contributes at most one plan to a wave;
- additional cells may enter later waves when their first unassigned plan can be placed;
- no existing cell must complete before another cell enters;
- a cell may skip a wave when its next plan conflicts with selected work;
- all work in the current wave finishes before the next wave begins.

Do not impose a fixed maximum number of active cells.
When contention becomes high, reduce new cell introduction or let conflicting cells skip a wave.

Among viable placements, prefer lighter cells when useful, without prescribing a detailed scoring algorithm.

Dependency semantics, dependency recording, plan state, and plan-readiness semantics belong to the ordinary plan workflow.
Cell-complete planning changes when repository-wide dependency analysis is performed, not what a dependency means.

When a wave is being fixed for implementation, cell-complete planning acts as the coordinating pre-execution gate for that wave.
Before fixing it, audit every assigned cell plan's settled completion contract and ensure every required test case and expected outcome is recorded through the ordinary planning workflow.
Do not fix or execute the wave while any required test definition is missing.
If that audit exposes an unresolved requirement, design ambiguity, invalid plan boundary, dependency change, or concurrency conflict, return it to its owning workflow before fixing the wave.

## Shared parts

Shared parts are not independent completion targets.

Shared implementation may be created or extracted while implementing currently needed cell behavior.
Do not implement extra shared behavior merely in anticipation of future cells.

A part created while one cell is incomplete may be reused by another cell, and the reusing cell may complete first.

Unchanged shared parts may be reused by multiple plans in the same wave.

When a cell plan changes a shared part, no other plan in that wave may read, depend on, or modify that shared part.
Represent that mutual exclusion as a concurrency conflict rather than an artificial dependency.

## Incomplete cells

Partial implementation of an incomplete cell may exist in the canonical repository.
The cell remains outside the formal completion guarantee until its completion conditions are satisfied.

Whether an incomplete cell itself is exposed or usable at runtime is repository-specific.

When a cell plan provisionally places an incomplete cell, that placement is a reduced form of the completed cell.
Every component present in the provisional cell also belongs to the completed cell with the same kind and role.
Components that are unnecessary at that point may be omitted.
Progress toward completion adds components instead of introducing provisional-only components or temporary substitutes that must later be replaced.
This constrains the components already present without requiring the cell's entire future component set to be fixed in advance.

## Temporary tests

Temporary tests are allowed while a cell is incomplete.

They may directly verify cell-specific internals, shared parts, or partial combinations needed to validate work in progress.

Temporary tests must be identifiable as temporary through a repository-appropriate convention.

When a cell becomes complete, move its durable behavioral guarantee to formal tests using real repository cells.
Remove temporary tests whose role has been transferred to that formal coverage.
Temporary tests that remain necessary for other incomplete-cell work may remain.

## Formal tests and completion

Formal tests verify observable required behavior at cell completion boundaries rather than freezing internal implementation structure.

Formal tests must use cells that actually exist as repository entities.
A cell currently being completed may be used in those tests before its completion status is finalized.
Do not create fictional or test-only cells merely to make a formal test convenient.

Mocks, stubs, fixtures, and similar test doubles may still be used for dependencies that are not cells.

A dedicated one-to-one test case or test file for every cell is not required.
The formal test suite as a whole may provide sufficient coverage.

When behavior emerges only from combining multiple cells, the cell implemented later owns the formal coverage that becomes applicable when it makes that combined behavior available.

A cell is complete only when:

- all behavior required of the cell is implemented;
- its observable required behavior is covered by formal tests using real repository cells, including the cell being completed;
- obsolete temporary tests have been removed; and
- the full automated test suite passes, preserving previously completed cells.

These conditions belong in the completion criteria of the cell plan that completes the cell so the ordinary plan-based implementation and review workflows can enforce them.

## Refactoring

Completed cells may be refactored internally, including extracting or reorganizing shared parts.

Completion is preserved when completed-cell behavior remains formally covered and the full automated test suite passes.

## Skill split

This repository provides two skills:

- planning: establish cell boundaries, plan cells locally with only intra-cell dependencies, then perform global dependency and conflict analysis to assign cell plans to current and future waves, and audit required test-definition completeness when fixing a wave for implementation;
- implementation: execute one planned wave after that pre-execution gate while preserving plan boundaries, shared-part exclusivity, temporary-test rules, and cell completion conditions.

A cell-specific review skill is unnecessary.
Review remains plan-based and is handled by the ordinary review workflow using the completion criteria created during cell-complete planning.
