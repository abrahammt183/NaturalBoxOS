# G-004 — BoxType

## Definition

A **BoxType** defines the kind of contents stored in a Box.

NaturalBoxOS defines exactly two BoxTypes:

- `<Architecture>`
- `etc`

Where `<Architecture>` is the target instruction set architecture of the Box (for example, `x86_64`, `aarch64`, or `riscv64`).

## Purpose

A BoxType determines whether a Box contains application files or configuration files.

## Related Terms

- G-001 — Box
- G-002 — boxInfo
- G-003 — BoxID

## Notes (Informative)

A Box whose BoxType is an architecture stores application files.

Application Boxes are stored under a `dep` directory.

A Box whose BoxType is `etc` stores configuration files.

Configuration Boxes are stored under an `etc` directory.

A Configuration Box may belong to an Application Box or may exist independently, such as a user-created Box for backing up a directory.
