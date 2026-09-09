# G-007 — ClosedBox

## Definition

A **ClosedBox** is a Box whose OuterBoxes are contained within the Box.

A ClosedBox is identified by the value `box = in` in its `boxInfo`.

## Purpose

A ClosedBox defines the boundary at which OuterBoxes are stored.

## Related Terms

- G-001 — Box
- G-002 — boxInfo
- G-006 — OpenBox

## Notes (Informative)

When BoxMaker places an OuterBox, it recursively traverses the ParentBox hierarchy until it reaches the first ClosedBox.

The OuterBox is created or reused within the `dep` directory of that ClosedBox.

Every Box hierarchy SHALL contain exactly one top-level ClosedBox.
