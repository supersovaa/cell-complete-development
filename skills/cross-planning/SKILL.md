---
name: cross-planning
description: Analyze cross-cell result dependencies and coarse implementation order when multiple planned cells need coordination, without assigning waves.
---

# Cross Planning

Use this skill when relationships among multiple locally planned cells need coordination. Analyze the relevant cells without waiting for every other cell to be planned or completed.

Use this skill together with the repository's ordinary plan-driven planning rules.
Those rules own dependency semantics, dependency recording, plan state, plan-readiness semantics, durable state, and general concurrency-conflict recording.
This skill owns cross-cell planning only.

## Analyze cell relationships

Read the planned results and intra-cell direct dependencies established by cell planning.
Compare the cells that are candidates for coordinated implementation.

Determine which results of one cell are required to establish results of another cell.
Record the resulting cross-cell direct dependencies on the affected implementation plans in the existing planning index using the ordinary dependency semantics.

Do not turn a preferred implementation order into a dependency.
A cross-cell dependency represents a required preceding result, not merely a convenient sequence.

Do not require one cell to complete before another cell starts when only a partial result of the first cell is required.
Depend on the specific plan result that establishes what the later plan needs.

## Establish coarse implementation order

Use the cross-cell dependency structure to establish a coarse implementation order among cells.
Use that order to identify which cells can begin independently, which cells become useful to introduce later, and which cells are constrained by results from other cells.

Keep this order coarse and revisable.
Do not assign wave numbers or require every plan of an earlier cell to precede every plan of a later cell.
Leave exact plan-level concurrency analysis and synchronized wave placement to wave planning.

When several orders satisfy the required dependencies, prefer an order that preserves opportunities for diagonal progress across multiple cells.
Do not invent dependencies merely to encode that preference.

Record the coarse order in an appropriate existing Markdown planning document, such as the implementation index. Keep this revisable guidance distinct from required direct dependencies; no special format or new file is needed.

## Return local problems upstream

If cross-cell analysis shows that a cell plan combines independently establishable results in a way that prevents the required cross-cell dependency from being expressed at the actual result boundary, return that cell to cell planning for decomposition.

If cross-cell analysis exposes an incorrect intra-cell dependency or an unresolved cell-local requirement, return the affected cell to its owning planning workflow. Keep unrelated plans available for wave arrangement.

This skill owns cross-cell result dependencies and coarse implementation order.
Cell-local decomposition, intra-cell dependencies, and cell-completion criteria belong to cell planning.
Plan-level concurrency analysis, wave assignment, and the pre-execution wave gate belong to wave planning.
General plan semantics belong to the ordinary planning workflow.
