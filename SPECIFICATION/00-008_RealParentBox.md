# G-008 — RealParentBox

## Definition

A **RealParentBox** is a Box that declares another Box as one of its dependencies in its `boxInfo`.

The RealParentBox is the logical owner and user of the child Box.

An InnerBox has exactly one RealParentBox.

An OuterBox may have one or more RealParentBoxes.

## Purpose

A RealParentBox defines the dependency relationships between Boxes.

## Related Terms

- G-001 — Box
- G-002 — boxInfo
- G-006 — OpenBox
- G-007 — ClosedBox
- G-009 — FosterParentBox
- G-010 — InnerBox
- G-011 — OuterBox
