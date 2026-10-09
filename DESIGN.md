# Cell-Complete Development Design

## Purpose

Cell-complete development treats a concrete domain entity as the minimum completion boundary while allowing implementation work to be decomposed more finely and to progress diagonally across multiple cells.

It is intended for domains that naturally contain moderately sized, clearly separable entities such as individual cards or characters.

## Cell

A cell is a concrete, individually identifiable domain entity whose completion can be judged independently.

The exact entity type that constitutes a cell is repository-specific.
Use an existing repository definition when available; otherwise establish the cell boundary before applying this method.

A cell is a completion and verification boundary, not necessarily a code-ownership, dependency, or implementation boundary.

## Planning responsibilities

Cell planning, cross planning, and wave planning are independently invocable: they handle one-cell decomposition, cross-cell coordination when needed, and plan-level wave arrangement, respectively. Unrelated eligible work can be arranged without waiting for all other planning to finish.

### Cell-local planning

Plan one cell at a time.

Decompose the cell into implementation plans that each establish one concrete partial result toward completing that cell.
One such plan belongs to exactly one cell.

During this pass, determine and record only the intra-cell subset of direct dependencies using the ordinary dependency semantics.
Do not treat the repository-wide direct-dependency set as complete yet.
Do not perform repository-wide dependency analysis, compare the cell with other cells, or assign its plans to waves.

A plan produced by this pass is wave-unassigned until global wave planning places it.
Wave assignment is separate from the ordinary durable plan state; wave-unassigned does not add a new plan state.
Persist each plan's cell membership and wave assignment in the repository's implementation index so wave planning can consume the cell-local result without rediscovering it.
Within the cell-complete workflow, a wave-unassigned plan is not selected for implementation.

Do not create an independent plan merely to implement a shared effect, helper, mechanism, or reusable abstraction.
A cell plan may create, change, or extract shared implementation when that work is required to establish the cell plan's result.

Keep enough future cell-local structure to guide later work, but do not invent plans whose boundaries cannot yet be established from available facts.

### Cross-cell planning

When coordination among multiple locally planned cells is needed, compare their planned results without waiting for every cell to be planned or completed.

Establish cross-cell direct dependencies at the specific plan-result boundary that another cell actually requires.
Do not convert a preferred implementation order into a dependency, and do not require a whole cell to complete when only one of its partial results is required.

Use those dependencies to establish a coarse, revisable implementation order among cells.
Identify cells that can begin independently, cells that become useful to introduce later, and cells constrained by results from other cells.
Preserve opportunities for diagonal progress when several orders satisfy the required dependencies.

Record cross-cell dependencies according to the ordinary planning index convention. Record the coarse implementation order in an appropriate existing Markdown planning document, such as that index, separately from required dependencies; no new file or format is needed.

Do not assign wave numbers during this pass.
If the analysis shows that a cell combines independently establishable results such that the required cross-cell dependency cannot be expressed at the actual result boundary, return that cell to cell planning without stopping unrelated work.

### Global wave planning

When arranging waves, inspect wave-unassigned and already future-assigned cell plans together with the repository-wide plan graph. Use available cross-cell findings where relevant.

Use the established direct dependencies as scheduling inputs. Before wave assignment, confirm required dependencies for the selected plans and finalize plan-level concurrency constraints, including conflicts involving shared implementation. If cross-cell relationships remain unclear for affected plans, invoke cross planning for those cells or leave their plans unassigned while arranging independent eligible work.

If analysis reveals an invalid boundary or dependency structure, leave the affected plan unassigned and return it to the owning planning workflow while continuing with unaffected eligible work.

Assign supported cell plans to synchronized waves and persist the assignments in the authoritative implementation index.
Plan multiple future waves when the known dependencies and conflicts are sufficient to do so.
A cell plan may remain explicitly wave-unassigned when its global placement is not yet justified.

Persist one authoritative status per wave indicating whether it is future or fixed.
Keep wave fixedness separate from per-plan wave assignment.
Keep waves beyond the next execution target in future status and fix only the earliest not-yet-completed wave selected for execution.
Future wave assignments remain revisable until their wave is fixed for implementation.
Only plans in a fixed wave are eligible for implementation through the cell-complete workflow.

A typical progression is:

- wave 1: A1;
- wave 2: A2 and B1;
- wave 3: A3, B2, and C1.

Here A, B, and C are cells, and each numbered item is a cell plan.

Use staircase-shaped diagonal progress as the default:

- a cell contributes at most one plan to a wave;
- additional cells may enter later waves when at least one of their wave-unassigned plans is eligible for placement;
- no existing cell must complete before another cell enters;
- a cell may skip a wave when none of its eligible plans can safely run with selected work;
- all work in the current wave finishes before the next wave begins.

Do not impose a fixed maximum number of active cells.
When contention becomes high, reduce new cell introduction or let conflicting cells skip a wave.

Among viable placements, prefer lighter cells when useful, without prescribing a detailed scoring algorithm.

Dependency semantics, dependency recording, plan state, and plan-readiness semantics belong to the ordinary plan workflow.
Cell planning establishes intra-cell dependencies; cross planning determines needed cross-cell dependencies and coarse order. Wave planning consumes established dependencies and completes concurrency analysis without changing ordinary dependency semantics.

When a wave is being fixed for implementation, wave planning acts as the coordinating pre-execution gate for that wave.
Before fixing it, use every assigned cell plan's settled completion contract as input to `test-evidence-planning`, or verify that an applicable settled result already exists.
Do not fix or execute the wave until the required test definitions and material testing decisions produced by `test-evidence-planning` are complete, persisted using repository conventions, and linked from each plan.
If that gate exposes an unresolved requirement, design ambiguity, invalid plan boundary, dependency change, or concurrency conflict, return it to its owning workflow before fixing the wave.

## Shared parts

Shared parts are not independent completion targets.

Shared implementation may be created or extracted while implementing currently needed cell behavior.
Do not implement extra shared behavior merely in anticipation of future cells.

A part created while one cell is incomplete may be reused by another cell, and the reusing cell may complete first.

Plans may read or reuse shared parts within the same wave, including while another plan changes them, when the planned uses remain compatible and independently establishable.

Assess shared-part interference at the concrete responsibility, interface, data contract, or intended edit boundary using settled plans and current repository facts.
Common files, modules, dependencies, or read access alone do not establish a concurrency conflict.
Independent changes can share a wave when their edits and behavioral contracts can be integrated without invalidating either plan's behavior, assumptions, or verification.

Record a concurrency conflict, rather than an artificial dependency, when grounded overlapping edits or incompatible changes make simultaneous implementation unsafe.
Use a direct dependency when one plan needs another plan's established result first.
Reassess earlier broad conflicts during wave planning based on settled plan results and contracts; open implementation choices alone do not establish conflicts. Return genuinely unresolved planning decisions to their owning workflow.

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

## Wave planning review

After a complete future wave arrangement exists, review it for suspicious serialization before treating the arrangement as efficient.

Let `C` be the number of covered cells, `Pmax` the largest number of plans in any one covered cell, and `W` the number of waves in the arrangement.
When `W > C + Pmax`, investigate whether avoidable serialization comes from cell-local decomposition, cross-cell relationships, or wave-level concurrency and placement.
Also distinguish overbroad concurrency constraints from genuine interference caused by responsibilities concentrated in shared implementation.
Treat this threshold as a diagnostic warning rather than a correctness condition, because required dependencies and genuine conflicts may justify additional waves.

Keep replanning and redesign under user control.
Report the evidence and the responsible planning layer or possible shared-implementation design concern rather than silently changing upstream plans or implementation during review.

## Refactoring

Completed cells may be refactored internally, including extracting or reorganizing shared parts.

Completion is preserved when completed-cell behavior remains formally covered and the full automated test suite passes.

## Skill split

This repository provides five skills:

- cell planning: establish one cell boundary, decompose that cell into plans, record intra-cell direct dependencies, and leave those plans wave-unassigned;
- cross planning: establish cross-cell result dependencies and record a revisable coarse implementation order when coordination is needed, without assigning waves;
- wave planning: use established dependencies, complete plan-level concurrency analysis, assign eligible cell plans to current and future waves without waiting for unrelated planning, and require completed `test-evidence-planning` when fixing a wave for implementation;
- wave planning review: diagnose suspicious serialization after wave arrangement and direct the user to the responsible planning layer or shared-implementation design evaluation;
- implementation: execute one fixed wave after that pre-execution gate while preserving plan boundaries, plan-level concurrency constraints, temporary-test rules, and cell completion conditions.

A cell-specific review skill is unnecessary.
Implementation-outcome review remains plan-based and is handled by the ordinary review workflow using the completion criteria created during cell planning. Wave planning review separately assesses the arrangement itself.
