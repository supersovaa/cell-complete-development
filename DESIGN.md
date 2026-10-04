# Cell-Complete Development Design

## Purpose

Cell-complete development treats a concrete domain entity as the minimum completion boundary while allowing implementation work to be decomposed more finely and to progress across multiple cells in parallel.

The method is intended for domains that naturally contain moderately sized, clearly separable entities such as individual cards or characters.

## Cell

A cell is a concrete, individually identifiable domain entity whose completion can be judged independently.

The exact kind of entity that constitutes a cell is repository-specific. If the repository already defines its cell boundary, use that definition. Otherwise, clarify the repository's cell definition before applying this method.

A cell is a completion and verification boundary, not necessarily a code ownership, dependency, or implementation boundary.

## Completion

A cell is complete only when:

- all behavior required of that cell is implemented;
- its observable required behavior is covered by formal tests using completed, real cells; and
- the full automated test suite passes, preserving the behavior of previously completed cells.

A dedicated one-to-one test case or test file for every cell is not required. The formal test suite as a whole may provide sufficient coverage.

## Implementation

Implementation may be decomposed below the cell boundary.

Multiple cells may be implemented in parallel. Work may progress diagonally across cells rather than completing one cell before touching the next.

Among implementable cells, prefer lighter cells when useful, without prescribing a detailed weighting or scheduling algorithm.

Do not treat a shared effect, mechanism, helper, or other reusable part as an independent completion target to implement ahead of cells.

Shared parts may be created or extracted while implementing currently needed cell behavior. Do not implement extra shared behavior merely in anticipation of future cells.

A part first created while implementing an incomplete cell may be reused by another cell. The originating cell does not need to complete first, and the reusing cell may complete earlier.

Partial implementation of an incomplete cell may exist in the canonical repository. It remains incomplete and outside the formal completion guarantee until the cell completion conditions are met.

Whether incomplete cells themselves are exposed or usable at runtime is repository-specific.

## Temporary tests

Temporary tests are allowed while a cell is incomplete.

They may directly test:

- internal cell-specific parts;
- shared parts;
- partial combinations needed to validate work in progress.

Temporary tests must be identifiable as temporary by a repository-appropriate convention such as naming, placement, or markers.

When a cell becomes complete, its required observable behavior must be covered by formal tests using completed cells. Temporary tests whose role has been transferred to formal cell-level verification should be removed rather than retained as permanent tests of internal implementation structure.

## Formal tests

Formal tests verify observable required behavior of completed cells rather than freezing internal implementation details.

Formal tests must use cells that actually exist as completed repository entities. Do not create fictional or test-only cells merely to make a formal test convenient.

Mocks, stubs, fixtures, and other test doubles may still be used for dependencies that are not themselves cells.

When behavior emerges only from combining multiple cells, add formal coverage once all participating cells are complete, using those completed cells.

Run the full automated test suite after changes. A newly completed cell is not complete if any existing formal test fails.

## Refactoring

Completed cells may be refactored internally, including extracting or reorganizing shared parts.

Refactoring preserves completion when the observable behavior of completed cells remains covered and the full automated test suite passes.

## Skill split

The repository will separate the method by use:

- planning: establish cell boundaries and guide cell selection and parallel progress;
- implementation: decompose work below cells, reuse parts, and use temporary tests while progressing toward completion;
- review: verify completion conditions, formal test coverage, removal of obsolete temporary tests, and regression safety.

Detailed skill wording remains to be settled through the design grill.
