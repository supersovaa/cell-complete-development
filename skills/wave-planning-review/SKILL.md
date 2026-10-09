---
name: wave-planning-review
description: Review a completed cell-wave arrangement for suspicious serialization and identify whether its cause lies in planning constraints or tightly coupled shared implementation.
---

# Wave Planning Review

Use this skill after wave planning has produced a complete future wave arrangement for a known set of planned cells.

This is a diagnostic review.
A suspicious wave count is evidence to investigate, not proof that the arrangement is invalid.

## Check the wave-count warning threshold

Let:

- `C` be the number of cells covered by the reviewed arrangement;
- `Pmax` be the largest number of implementation plans belonging to any one of those cells; and
- `W` be the number of waves required by the reviewed arrangement.

When `W > C + Pmax`, treat the arrangement as having a high likelihood of avoidable serialization and investigate why the extra waves are required.

Do not reject an arrangement solely because it exceeds this threshold.
Dependencies and genuine concurrency conflicts may justify a larger wave count.
Inspect a suspicious pattern of isolated shared-part plans even below the threshold when the arrangement itself suggests avoidable serialization.

## Locate the source of avoidable serialization

Inspect the causes that prevent plans from sharing earlier waves.
For each suspicious concurrency conflict, look for concrete incompatible edits, changed contracts, or invalidated implementation and verification assumptions.
A shared file, module, dependency, or read access alone is not enough to justify separating plans; check recorded conflicts against current plan boundaries rather than inheriting broad exclusions unexamined.
Distinguish simultaneous-implementation interference from a true prerequisite result that requires a direct dependency.

When a cell contains multiple independently establishable and verifiable results inside one plan, direct the user back to cell planning.

When cross-cell dependencies or the coarse implementation order are stronger than the required result relationships support, direct the user back to cross planning.

When plan-level concurrency conflicts or wave placement are more restrictive than established plan-level interference justifies, direct the user back to wave planning.

When genuine concurrency interference comes from independently meaningful responsibilities concentrated in one shared implementation concept, report the coupling as a possible design concern separately from conflict-label or wave-placement errors.
Recommend evaluating the shared concept through established meaning and whole-path simplification, referring to `concept-introduction-threshold` when available without requiring it to complete this review.
Preserve confirmed concurrency constraints unless a settled redesign removes their cause.

If the observed wave count is justified by required dependencies and genuine conflicts, report that conclusion without requiring replanning.

## Keep replanning under user control

Report the evidence for suspected inefficiency, the planning layer responsible for it, and the expected effect of reconsidering that layer.
Ask the user to return to the identified upstream planning layer rather than silently rewriting upstream plans during review.

This skill owns only diagnostic review of the completed wave arrangement.
It does not create cell plans, establish cross-cell dependencies, assign waves, or modify the reviewed planning records.
