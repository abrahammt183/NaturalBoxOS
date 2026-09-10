# G-010 — InnerBox

## Definition

An **InnerBox** is a child Box whose RealParentBox and FosterParentBox are the same Box.

An InnerBox is created when its RealParentBox is a ClosedBox or it's box field.
is set to "in".
## Purpose

An InnerBox is private to its RealParentBox and is not shared with other Boxes.

## Related Terms

- G-006 — OpenBox
- G-007 — ClosedBox
- G-008 — RealParentBox
- G-009 — FosterParentBox
- G-011 — OuterBox
