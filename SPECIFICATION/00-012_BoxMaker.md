# G-012 — BoxMaker

## Definition

**BoxMaker** is the Makefile of NaturalBoxOS.

It defines how Boxes are created, rebuilt, updated, boxed, unboxed, shared, and removed according to `boxInfo`.

BoxMaker is executed by the standard GNU `make` program.

## Purpose

BoxMaker describes the rules that transform the filesystem into the state defined by `boxInfo`.

## Related Terms

- G-001 — Box
- G-002 — boxInfo
- G-003 — BoxID
- G-008 — RealParentBox
- G-009 — FosterParentBox
- G-014 — Boxing
- G-015 — Unboxing

## Notes (Informative)

BoxMaker treats `boxInfo` as the sole source of truth.

The filesystem is modified to match `boxInfo`, never the reverse.
