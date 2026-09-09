# G-006 — OpenBox

## Definition

An **OpenBox** is a Box that puts its dependencies outsidei of the Box so they can be shared with other Boxes.

A dependency placed outside the Box is called an **OuterBox**.

An OpenBox is identified by the value `box = out` in its `boxInfo`.

## Purpose

An OpenBox allows compatible dependencies to be shared with other Boxes.

## Related Terms

- G-001 — Box
- G-002 — boxInfo
- G-007 — ClosedBox
- G-010 — OuterBox
