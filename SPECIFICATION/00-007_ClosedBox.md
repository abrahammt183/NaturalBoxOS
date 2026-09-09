# G-007 — ClosedBox

## Definition

A **ClosedBox** is a Box that keeps its dependencies inside the Box.

A dependency kept inside the Box is called an **InnerBox**.

A ClosedBox is identified by the value `box = in` in its `boxInfo`.

## Purpose

A ClosedBox keeps its dependencies private to the Box.

## Related Terms

- G-001 — Box
- G-002 — boxInfo
- G-006 — OpenBox
- G-009 — InnerBox
