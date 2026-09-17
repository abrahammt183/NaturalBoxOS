NaturalBoxOS — Specification: Glossary (G-001 – G-023)

G-001 — Box
The fundamental unit of NaturalBoxOS. A directory containing a boxInfo file. A package-Box additionally has the standard internal structure (bin/, lib/, etc/, dep/, sbin/); an etc-Box has no required structure beyond its own boxInfo. Every application, every configuration, and the system itself are all Boxes.

G-002 — boxInfo
The tab-separated metadata file that defines a Box. Columns: name type author version link box path description. The first row is the Box's own identity (the meta-package row); every following row declares one dependency the Box requires. On a user-authored (pre-install) boxInfo, only name/type/author/version/link/box need be filled for dependency rows — path and description are filled in by BoxMaker once installed.

G-003 — BoxID
The unique identifier for a Box: ${architecture}_${author}_${name}_${version} for a package-Box, ${link}_${author}_${name}_${version} for an etc-Box. BoxID is used both as the BoxRack archive filename, and to build a Box's physical placement path — ${path-to-foster-parent}/${architecture}/${author}/${name}/${version} — which is written into the path column of the dependency row in its RealParent's boxInfo.

G-004 — BoxType
The classification of a Box read from its type column: an architecture-specific package-Box (x86_64, arm64, riscv, or ? for "use running CPU arch") or an etc-Box (etc). RootBox is itself a package-Box, not a separate BoxType.

G-005 — BoxState
A Box's state, always derived (never stored), evaluated relative to what its RealParent's boxInfo says about it:

Found — the Box exists at the path its RealParent's path column declares for it.
Missing — the Box does not exist at the path its RealParent's path column declares for it.
Orphan — the Box has no RealParent at all (nothing in the logical hierarchy references it).
G-006 — RootBox
The top-level Box that contains all other Boxes. Any Box with no RealParent falls into one of three cases, decided purely by its location:

real-root-Box — it is / itself (there is only ever one).
orphan-Box — it has no RealParent but sits inside /dep (see BoxState G-005 / BoxBurning G-021 — this is what BoxBurning removes).
virtual-root-Box — it has no RealParent and sits anywhere outside /dep other than / itself — e.g. /home/user/project1 — creating a self-contained virtual environment for a specific project.
G-007 — ParentBox
Two distinct kinds of parent relationship exist for a given Box:

RealParent — the Box that has an actual dependency row for it in its boxInfo, and is the one that actually uses it.
FosterParent — a ClosedBox that physically keeps the Box inside its own dep/ folder. Determined (and changed) by the foster-homing process.
G-008 — ChildBox
A Box that appears as a dependency row in another Box's boxInfo (the counterpart to ParentBox — splits into real-child/foster-child the same way ParentBox does).

G-009 — InnerBox
A Box whose RealParent and FosterParent are the same Box — i.e. it stays physically inside its RealParent's dep/. This happens for either of two reasons:

the RealParent is itself a ClosedBox, or
the Box itself is declared ClosedBox (box=in) on its own dependency row within the RealParent's boxInfo.
G-010 — OuterBox
A Box whose RealParent is an OpenBox and whose own dependency row (in the RealParent's boxInfo) is also declared OpenBox (box=out). Such a Box is placed at the first ClosedBox found while walking the parent chain upward from its RealParent toward the RootBox — that ClosedBox becomes its FosterParent. BoxMaker calculates this placement during foster-homing and writes the resulting path back into the RealParent's boxInfo.

G-011 — CurrentBox
Context-dependent:

When a user executes a file belonging to a Box, CurrentBox is the Box that owns that file — the system reads that Box's boxInfo to resolve its dependencies.
When a user invokes BoxMaker or BoxKeeper, CurrentBox is whichever Box's folder $PWD is inside.
G-012 — OpenBox
The box field holds a dual, recursive meaning within a single boxInfo:

On the first row (a Box's own identity row), it's the gatekeeper: out means the Box lets its own dependencies pass outward through it during their foster-homing; in keeps them contained.
On any later row (a dependency declaration), it's that dependency-Box's own wish: out means that dependency-Box wants to go out (be fostered further up); in means it wants to stay in. That dependency-Box has its own boxInfo, with the same first-row/later-row rule applying recursively within it.
A Box is OpenBox when its box field (in whichever of the two roles above applies) is out.

G-013 — ClosedBox
The same field as G-012, but in: a Box's own gatekeeper set to in stops things from passing out through it; a dependency row set to in means that dependency wants to stay with its RealParent.

G-014 — BoxMaker
In practice a makefile.txt driven by make, used to create, update, back up, and restore a single Box — the CurrentBox.

G-015 — BoxKeeper
A bash script. On invocation it first locates the RootBox to work from, using $PWD:

If $PWD starts with /dep, or there is no boxInfo in $PWD at all → the RootBox is / (real-root-Box).
Otherwise ($PWD does not start with /dep and a boxInfo exists there): read that boxInfo's first line (its BoxID). If the name of the parent (..) folder does not equal that Box's version, $PWD doesn't fit the standard dep/.../${version}/ nesting shape — so $PWD itself is the virtual-root-Box. If the parent folder's name does equal the version (meaning $PWD looks like it's nested inside a normal dependency tree), BoxKeeper moves up to that Box's FosterParent and repeats the check, continuing until it finds the virtual-root-Box.
Once the RootBox is located, it: 3. Walks every boxInfo file one by one following the logical hierarchy (the RealParent/ChildBox relations declared inside boxInfo files — not the physical/real folder layout), building a table of that logical hierarchy called the Kardex. 4. If it finds any mismatch or missing property, it has BoxMaker fix it (this phase is BoxKeeping). 5. Once existing Boxes in the logical hierarchy are fixed, it scans the real hierarchy (actual folders on disk) for Boxes absent from the Kardex — orphans — and deletes them (this phase is BoxBurning).

G-016 — BoxKeeping
The reconciliation phase of running BoxKeeper: traversing the logical hierarchy, building the Kardex, and having BoxMaker fix any Box found mismatched or missing. (BoxKeeper performs both BoxKeeping and BoxBurning, in that order.)

G-017 — BoxMaking
One of BoxMaker's jobs: fetching files from repositories, placing them into a Box, foster-homing it, and editing boxInfo files. (Distinct from Boxing and Unboxing, below — BoxMaking is what happens when there's no existing backup to draw on.)

G-018 — Boxing
The process of making a backup of a Box (archiving it to .box.tar.xz in BoxRack).

G-019 — Unboxing
The process of restoring a backup file — this happens when a boxInfo dependency row is satisfied by an existing backup file rather than needing a fresh fetch.

G-020 — BoxSending
Sending a log file and one or more .tar.xz archives from a sender to a receiver. The receiver places the .tar.xz file(s) into /boxRack and the log file(s) into /boxMaker/logs, then edits the boxInfo where they want that archive used, adding a dependency row with the archive's BoxID. Calling BoxMaker on that Box then triggers Unboxing: BoxMaker finds the log and archive, foster-homes the new Box, makes its folder, extracts (tar xvf) the archive into it, and sets the path/description fields on that dependency row in the RealParent's boxInfo.

(/boxMaker is the general home for all of BoxMaker's and BoxKeeper's own files — /boxMaker/logs is one folder within it.)

G-021 — BoxBurning
The process, run by BoxKeeper after BoxKeeping, of finding orphan-Boxes (present in the real/physical hierarchy but absent from the Kardex) and deleting them.

G-022 — BoxRack
The single flat folder at the system root (/boxRack) where every Box's archive (.box.tar.xz) is stored — package-Boxes and etc-Boxes alike — with no subfolders; uniqueness comes entirely from the filename.

G-023 — Kardex
A temporary table built by BoxKeeper, representing the relations of all boxInfo files across the logical hierarchy. Used for both foster-homing and BoxBurning.
