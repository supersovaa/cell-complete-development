---
name: wave-planning
description: Arrange eligible cell plans into waves using dependencies, concurrency analysis, and grounded integration-effort tradeoffs, then require settled test-evidence planning before fixing a wave.
---

# Wave Planning

Use this skill when one or more cell plans need wave arrangement or a wave needs fixing. Use available cross-planning findings where relevant; cross planning is not a prerequisite for a single cell or unrelated eligible plans.

Use this skill together with the repository's ordinary plan-driven planning rules.
Those rules own dependency semantics, dependency recording, plan state, plan-readiness semantics, durable state, and general concurrency-conflict recording.
This skill owns plan-level concurrency analysis and repository-wide wave arrangement for cell-complete work.

## Complete plan-level concurrency analysis

Read cell membership, recorded direct dependencies, any available coarse cell implementation order, and wave assignments from the existing planning records.
Inspect wave-unassigned and already future-assigned cell plans together with the repository-wide plan graph.

Before assigning a plan to a wave, confirm that its required direct dependencies are known and finalize its concurrency constraints using the ordinary planning workflow, including conflicts involving shared implementation.
Reassess recorded conflicts against current plan boundaries and repository facts before using them to exclude a same-wave placement; an earlier conflict label alone is not sufficient evidence.
When coordination among several cells reveals an unresolved cross-cell dependency, use cross planning for the affected cells or leave their plans unassigned. Continue arranging independent eligible plans without waiting for unrelated planning to finish.
Use established cell-local and cross-cell dependencies as scheduling inputs without inventing extra ordering constraints.

If concurrency analysis exposes an unresolved requirement or invalid upstream planning result, leave the affected work unassigned and return the issue to its owning planning workflow. Continue with unaffected eligible plans.

## Arrange current and future waves

Assign supported cell plans to synchronized waves and persist each assignment in the same authoritative record used for cell membership.
Plan multiple future waves when known dependencies and conflicts support those placements.
Keep an explicit unassigned value for a cell plan whose global placement is not yet justified.

Record each wave's status as future or fixed using the repository's existing wave convention; when none exists, keep one authoritative status record per wave in the implementation index.
Do not duplicate wave fixedness as mutable per-plan state.
Keep assigned waves after the next execution target in future status.
Fix only the earliest not-yet-completed wave selected for execution.
Future wave assignments may be revised until their wave is fixed for implementation.
Only plans in a fixed wave are eligible for implementation through the cell-complete workflow.

Use staircase-shaped diagonal progress when dependencies or concrete conflicts require staggered placement:

- plans from the same cell may share a wave when no direct dependency or concurrency conflict prevents simultaneous execution;
- additional cells may enter later waves when at least one of their wave-unassigned plans is eligible for placement;
- no existing cell must complete before another cell enters;
- a cell may skip a wave when none of its eligible plans can safely run in that wave;
- a wave completes before the next wave begins.

Use ordinary plan state and dependency results as inputs to wave construction rather than redefining plan readiness or dependency semantics here.

Place eligible plans in the earliest revisable wave supported by their dependencies and concrete concurrency conflicts. Defer an otherwise placeable plan only for a specific, evidenced constraint or a material integration-effort benefit established by the comparison below; record the reason in existing planning records and keep execution-capacity limits distinct from plan conflicts.
Among placements that require a choice, prefer lighter cells when useful, without a fixed scoring system or maximum number of active cells.

## Assess shared-part interference while arranging waves

Plans may read or reuse shared implementation in the same wave, including when another plan changes it, if the planned use relies on compatible contracts and their results remain independently establishable.

Assess interactions at the narrowest concrete shared responsibility, interface, data contract, or intended edit supported by the plans and current repository facts.
Check whether a proposed change can invalidate another plan's edits, required behavior, implementation assumptions, or verification.
Sharing a file, module, or dependency, or reading a shared part, does not by itself establish a conflict.
Assess implementation choices left open by the plan through their settled results and contracts rather than assuming that possible edit locations will conflict.

Allow independent changes in the same wave when their edits and behavioral contracts can be integrated without invalidating one another.
Record a concurrency conflict when a concrete, plausible interference would make simultaneous implementation unsafe, and identify the interaction that justifies the constraint.
Use a direct dependency instead when one plan requires the established result of another before implementation.

When concrete concurrency conflicts prevent co-placement, keep safe parallelism by skipping conflicting cells or delaying additional cell introduction.
Do not force every active cell to advance in every wave.

## Compare integration effort for otherwise safe placements

When plans are independently completable in one wave but a planned change to a shared contract or execution path would require several other plans to adapt, compare co-placement with establishing the shared result in an earlier wave.

Ground both alternatives in settled plan boundaries and repository facts. Identify the producing and consuming plans, the affected contract or behavior, and likely work to synchronize branches, reconcile edits, adapt consumers and their test expectations, and rerun integration verification. Balance those costs against useful parallel progress, added waiting, and extra wave or integration work under the staged alternative.

Keep integration-effort choices distinct from direct result dependencies and concurrency conflicts. Select a later revisable wave for an otherwise eligible plan when the concrete comparison supports a material net reduction in work; otherwise retain its earliest supported placement. Shared file counts and a final conflict-free merge are only partial evidence of the effort incurred.

Record the compared placements, evidence, expected tradeoff, and selected order in the existing planning record. Apply the comparison to affected plans while unrelated eligible work continues.

## Require test-evidence planning when fixing a wave

Before fixing a wave for implementation, inspect every cell plan assigned to that wave.
Use each plan's settled completion contract as the bounded input to `test-evidence-planning`, or verify that an applicable settled test-evidence planning result already exists.

Require the resulting test definitions and material testing decisions to be complete, recorded using repository conventions, and linked from each plan before fixing the wave.
Ensure the planning scope includes deferred formal coverage that cell planning assigned to a completing plan when its realization condition is present.
Deferred formal coverage whose realization condition is still absent remains outside the current wave's required test-evidence planning scope.

Do not fix the wave or begin its implementation while required test-evidence planning is incomplete.

If test-evidence planning exposes an unresolved requirement or design ambiguity, return that issue to its owning workflow.
If the gate exposes an invalid plan boundary, dependency change, or concurrency conflict, return that issue to its owning planning workflow before fixing the wave.

This skill owns plan-level concurrency completion, evidence-based placement tradeoffs, wave assignment, and the pre-execution gate that requires completed `test-evidence-planning` for a fixed wave.
Cell-local decomposition and cell-completion criteria belong to cell planning.
Cross-cell result dependencies and coarse cell implementation order, when coordination across cells is needed, belong to cross planning. Completing a separate cross-planning pass is not a gate for unrelated wave arrangement.
Required test cases and expected outcomes belong to `test-evidence-planning`.
Wave execution belongs to cell-complete implementation.
General plan semantics belong to the ordinary planning workflow.
