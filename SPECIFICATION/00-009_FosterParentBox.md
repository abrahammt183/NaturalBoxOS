# G-009 — FosterParentBox

## Definition

A **FosterParentBox** is the Box that physically stores a child Box.

Every child Box has exactly one FosterParentBox.

The FosterParentBox is determined by the physical location of the child Box.

## Purpose

A FosterParentBox provides the physical location where a child Box is stored, shared, boxed, and unboxed.

## Related Terms

- G-001 — Box
- G-002 — boxInfo
- G-006 — OpenBox
- G-007 — ClosedBox
- G-008 — RealParentBox
- G-010 — InnerBox
- G-011 — OuterBox

## Notes (Informative)

A FosterParentBox does not record its child Boxes in its `boxInfo`.

The physical location of a child Box may change when BoxMaker reorganizes the Box hierarchy according to the boxInfo of its RealParentBox, its FosterParentBox, or any Box in the ParentBox chain between them.
