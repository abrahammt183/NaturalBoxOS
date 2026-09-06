# G-002 — boxInfo

## Definition

`boxInfo` is the metadata file that defines a Box.

Every Box SHALL contain exactly one file named `boxInfo`.

A directory that does not contain a `boxInfo` file is not a Box.

## Purpose

`boxInfo` defines the identity, properties, and relationships of its Box.

It is the authoritative source of metadata for the Box.

## Related Terms

- G-001 — Box
- G-003 — BoxID
- G-004 — BoxType
- G-005 — BoxState

## Notes (Informative)

The internal format and contents of `boxInfo` are defined elsewhere in this specification.
