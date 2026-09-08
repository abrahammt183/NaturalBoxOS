# G-005 — BoxState

## Definition

A **BoxState** describes the current condition of a Box.

A BoxState reflects whether a Box is consistent with its `boxInfo` and with the NaturalBoxOS Box hierarchy.

## Purpose

BoxState enables BoxKeeper to determine whether a Box requires any action.

## Related Terms

- G-001 — Box
- G-002 — boxInfo
- G-003 — BoxID
- G-015 — BoxKeeper

## Notes (Informative)

Examples of BoxStates include:

- Valid
- Missing
- Broken
- Orphan
- Inconsistent

The complete definition of each BoxState and the conditions under which it occurs are specified elsewhere in this specification.
