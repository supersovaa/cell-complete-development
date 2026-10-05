# Cell-Complete Development Design

## Purpose

Cell-complete development treats a concrete domain entity as the minimum completion boundary while allowing implementation work to be decomposed more finely and to progress diagonally across multiple cells.

It is intended for domains that naturally contain moderately sized, clearly separable entities such as individual cards or characters.

## Cell

A cell is a concrete, individually identifiable domain entity whose completion can be judged independently.

The exact entity type that constitutes a cell is repository-specific.
Use an existing repository definition when available; otherwise establish the cell boundary before applying this method.

A cell is a completion and verification boundary, not necessarily a code-ownership, dependency, or implementation boundary.

## Cell stages and plans

Implementation may be decomposed below the cell boundary.

Each cell is advanced through stages.
Each stage establishes one concrete partial result toward completing that cell and corresponds one-to-one with one implementation plan.

One plan belongs to exactly one stage of exactly one cell.

Do not create an independent plan merely to implement a shared effect, helper, mechanism, or reusable abstraction.
A cell-stage plan may create, change, or extract shared implementation when that work is required to establish its stage result.

Keep enough future stage structure to guide implementation, but do not freeze the complete stage sequence in advance.
Later stages may be revised as earlier work establishes new facts.
Near-term plan fixation remains the responsibility of the ordinary plan workflow.

## Diagonal waves

Arrange cell-stage plans into synchronized waves.

A typical progression is:

- wave 1: A1;
- wave 2: A2 and B1;
- wave 3: A3, B2, and C1.

Here A, B, and C are cells, and each numbered item is a stage plan.

Use staircase-shaped diagonal progress as the default:

- an active cell advances by at most one stage in a wave;
- new cells may enter whenever their first stage is viable;
- no existing cell must complete before another cell enters;
- a cell may skip a wave when its next stage conflicts with selected work;
- all work in the current wave finishes before the next wave begins.

Do not impose a fixed maximum number of active cells.
When contention becomes high, reduce new cell introduction or let conflicting cells skip a wave.

Among viable cells, prefer lighter cells when useful, without prescribing a detailed scoring algorithm.

Dependency semantics, dependency recording, and plan readiness belong to the ordinary plan workflow.
Cell-complete planning consumes those results when constructing waves.

## Shared parts

Shared parts are not independent completion targets.

Shared implementation may be created or extracted while implementing currently needed cell behavior.
Do not implement extra shared behavior merely in anticipation of future cells.

A part created while one cell is incomplete may be reused by another cell, and the reusing cell may complete first.

Unchanged shared parts may be reused by multiple plans in the same wave.

When a stage plan changes a shared part, no other plan in that wave may read, depend on, or modify that shared part.
Represent that mutual exclusion as a concurrency conflict rather than an artificial dependency.

## Incomplete cells

Partial implementation of an incomplete cell may exist in the canonical repository.
The cell remains outside the formal completion guarantee until its completion conditions are satisfied.

Whether an incomplete cell itself is exposed or usable at runtime is repository-specific.

Before an incomplete cell is provisionally placed, its completed component composition must be determined by the settled requirements and design.
If that composition is not yet settled, resolve it in the workflow that owns the missing requirement or design decision before planning provisional placement.
The provisional cell is a reduced form of that completed composition: omit components that are unnecessary at the current stage.
Components that remain keep the same kind and role they have in the completed composition.
Progress toward completion adds omitted components instead of replacing placeholder-only components or temporary substitutes.
Using the completed composition does not freeze the cell's complete future stage sequence.

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

- its component composition matches the completed composition required by the settled requirements and design;
- all behavior required of the cell is implemented;
- its observable required behavior is covered by formal tests using real repository cells, including the cell being completed;
- obsolete temporary tests have been removed; and
- the full automated test suite passes, preserving previously completed cells.

These conditions belong in the completion criteria of the cell's final stage plan so the ordinary plan-based implementation and review workflows can enforce them.

## Refactoring

Completed cells may be refactored internally, including extracting or reorganizing shared parts.

Completion is preserved when completed-cell behavior remains formally covered and the full automated test suite passes.

## Skill split

This repository provides two skills:

- planning: establish cell boundaries, decompose cells into stage plans, and construct conflict-safe diagonal waves;
- implementation: execute one planned wave while preserving plan boundaries, shared-part exclusivity, temporary-test rules, and cell completion conditions.

A cell-specific review skill is unnecessary.
Review remains plan-based and is handled by the ordinary review workflow using the completion criteria created during cell-complete planning.
