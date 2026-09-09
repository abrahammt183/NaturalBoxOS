# G-006 — OpenBox

## Definition

An **OpenBox** is a Box whose OuterBoxes are placed outside the Box.

An OpenBox is identified by the value `box = out` in its `boxInfo`.

## Purpose

An OpenBox allows compatible dependency Boxes to be shared with other Boxes through a common ancestor.

## Related Terms

- G-001 — Box
- G-002 — boxInfo
- G-005 — BoxState
- G-007 — ClosedBox

## Notes (Informative)

An OpenBox does not determine the final location of an OuterBox.

The location of an OuterBox is determined by BoxMaker according to the Dependency Placement Algorithm defined elsewhere in this specification.
