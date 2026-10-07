---
name: cell-planning
description: Plan one concrete domain cell at a time by decomposing it into implementation plans and recording only the direct dependencies within that cell, leaving every resulting plan unassigned to a wave.
---

# Cell Planning

Use this skill when a repository defines concrete domain entities as cells and one cell needs to be decomposed without performing repository-wide scheduling.

Use this skill together with the repository's ordinary plan-driven planning rules.
Those rules own plan structure, dependency semantics, plan-readiness semantics, durable state, and general concurrency-conflict recording.
This skill owns the cell-local planning phase only.

## Establish the cell boundary

Use the repository's existing definition of a cell when one exists.
Otherwise establish which concrete, individually identifiable domain entity is the current cell before planning it.

Treat the cell as a completion and verification boundary, not as a required code-ownership or dependency boundary.

## Decompose one cell

Decompose the current cell into implementation plans that each establish one concrete partial result toward completing that cell.
Each such plan belongs to exactly one cell.

Determine and record only the intra-cell subset of direct dependencies using the ordinary dependency semantics.
Leave repository-wide direct-dependency and concurrency analysis to wave planning.

Leave every plan produced by this phase wave-unassigned.
Wave assignment is separate from durable plan state; wave-unassigned does not add a new plan state.

Persist each plan's cell membership and explicit unassigned wave assignment using the repository's implementation-index convention.
When no repository-specific location exists, record them in the nearest common implementation index that already identifies the plans.
Keep one authoritative cell membership and wave-assignment record per plan.

Within the cell-complete workflow, keep a wave-unassigned plan out of implementation selection until wave planning completes repository-wide analysis and fixes a wave containing it.

## Keep shared implementation subordinate to the cell

Do not create a cell-independent plan merely to implement a shared effect, helper, mechanism, or reusable abstraction.
A cell plan may create, change, or extract shared implementation when that change is required to establish the plan result.

When a plan provisionally places an incomplete cell, plan it as a reduced form of the completed cell.
Every component included in the provisional cell must also belong to the completed cell with the same kind and role.
Omit components that are unnecessary at that point, and plan later work to extend the cell by adding components rather than replacing provisional-only components or temporary substitutes.

Keep enough future cell-local structure to guide implementation, but do not invent plan boundaries that available facts do not yet support.

## Inherit deferred formal coverage

When planning a cell, inspect unresolved deferred formal coverage recorded by the repository that could become observable through this cell.

If completing this cell would satisfy a recorded realization condition, assign that deferred coverage to this cell's completing plan. Once assigned, treat it as required observable behavior for completion rather than leaving it deferred because it originated in earlier work.

Do not assign deferred coverage whose realization condition still cannot be satisfied by real repository cells in the planned state. Such coverage remains deferred and does not block completion of unrelated cells.

When the current cell itself requires behavior that cannot be made observable by real repository cells available at completion, allow the cell to complete without fictional or test-only cells only when the applicable domain workflow records that gap as deferred formal coverage.

## Plan cell completion

The cell plan that completes a cell must make cell completion explicit.

Its completion criteria must require:

- all behavior required of the cell to be implemented;
- the cell's observable required behavior, including inherited deferred formal coverage that has become realizable, to be covered by formal tests using real repository cells, including the cell being completed;
- obsolete temporary tests whose role has moved to formal cell-level verification to be removed;
- newly established behavior that depends on combinations of cells to receive formal coverage in the completing plan of the cell implemented later, when that cell makes the combined behavior available; and
- the full automated test suite to pass.

Formal coverage does not require one dedicated test case or test file per cell.

Do not require fictional or test-only cells for formal tests.
Mocks, stubs, fixtures, and similar test doubles remain available for dependencies that are not cells.

This skill owns one-cell decomposition, intra-cell direct dependencies, and cell-completion criteria.
Repository-wide dependency and concurrency analysis, wave assignment, and the pre-execution wave gate belong to wave planning.
General plan semantics belong to the ordinary planning workflow.
