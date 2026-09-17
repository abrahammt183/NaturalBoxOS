NaturalBoxOS — Specification: Glossary (G-001 – G-023)
Status: All 23 terms confirmed.

Naming convention: literal file/folder/command names (e.g. boxInfo, boxMaker, /boxRack) are written exactly as they exist on disk, in code font, wherever they appear. Everywhere else — headings and prose describing the concept — compound terms are hyphenated (e.g. root-box, box-ID, box-maker).

G-001 — box
The fundamental unit of NaturalBoxOS. A directory containing a boxInfo file. A package-box additionally has the standard internal structure (bin/, lib/, etc/, dep/, sbin/); an etc-box has no required structure beyond its own boxInfo. Every application, every configuration, and the system itself are all boxes.

G-002 — box-info
The tab-separated metadata file, boxInfo, that defines a box. Columns: name type author version link box path description. The first row is the box's own identity (the meta-package row); every following row declares one dependency the box requires. On a user-authored (pre-install) boxInfo, only name/type/author/version/link/box need be filled for dependency rows — path and description are filled in by box-maker once installed.

G-003 — box-ID
The unique identifier for a box: ${architecture}_${author}_${name}_${version} for a package-box, ${link}_${author}_${name}_${version} for an etc-box. box-ID is used both as the box-rack archive filename, and to build a box's physical placement path — ${path-to-foster-parent}/${architecture}/${author}/${name}/${version} — which is written into the path column of the dependency row in its real-parent's boxInfo.

G-004 — box-type
The classification of a box read from its type column: an architecture-specific package-box (x86_64, arm64, riscv, or ? for "use running CPU arch") or an etc-box (etc). root-box is itself a package-box, not a separate box-type.

G-005 — box-state
A box's state, always derived (never stored), evaluated relative to what its real-parent's boxInfo says about it:

found — the box exists at the path its real-parent's path column declares for it.
missing — the box does not exist at the path its real-parent's path column declares for it.
orphan — the box has no real-parent at all (nothing in the logical hierarchy references it).
G-006 — root-box
The top-level box that contains all other boxes. Any box with no real-parent falls into one of three cases, decided purely by its location:

real-root-box — it is / itself (there is only ever one).
orphan-box — it has no real-parent but sits inside /dep (see box-state G-005 / box-burning G-021 — this is what box-burning removes).
virtual-root-box — it has no real-parent and sits anywhere outside /dep other than / itself — e.g. /home/user/project1 — creating a self-contained virtual environment for a specific project.
G-007 — parent-box
Two distinct kinds of parent relationship exist for a given box:

real-parent — the box that has an actual dependency row for it in its boxInfo, and is the one that actually uses it.
foster-parent — a closed-box that physically keeps the box inside its own dep/ folder. Determined (and changed) by the foster-homing process.
G-008 — child-box
A box that appears as a dependency row in another box's boxInfo (the counterpart to parent-box — splits into real-child/foster-child the same way parent-box does).

G-009 — inner-box
A box whose real-parent and foster-parent are the same box — i.e. it stays physically inside its real-parent's dep/. This happens for either of two reasons:

the real-parent is itself a closed-box, or
the box itself is declared closed-box (box=in) on its own dependency row within the real-parent's boxInfo.
G-010 — outer-box
A box whose real-parent is an open-box and whose own dependency row (in the real-parent's boxInfo) is also declared open-box (box=out). Such a box is placed at the first closed-box found while walking the parent chain upward from its real-parent toward the root-box — that closed-box becomes its foster-parent. box-maker calculates this placement during foster-homing and writes the resulting path back into the real-parent's boxInfo.

G-011 — current-box
Context-dependent:

When a user executes a file belonging to a box, current-box is the box that owns that file — the system reads that box's boxInfo to resolve its dependencies.
When a user invokes box-maker or box-keeper, current-box is whichever box's folder $PWD is inside.
G-012 — open-box
The box field holds a dual, recursive meaning within a single boxInfo:

On the first row (a box's own identity row), it's the gatekeeper: out means the box lets its own dependencies pass outward through it during their foster-homing; in keeps them contained.
On any later row (a dependency declaration), it's that dependency-box's own wish: out means that dependency-box wants to go out (be fostered further up); in means it wants to stay in. That dependency-box has its own boxInfo, with the same first-row/later-row rule applying recursively within it.
A box is open-box when its box field (in whichever of the two roles above applies) is out.

G-013 — closed-box
The same field as G-012, but in: a box's own gatekeeper set to in stops things from passing out through it; a dependency row set to in means that dependency wants to stay with its real-parent.

G-014 — box-maker
In practice a makefile.txt driven by make, used to create, update, back up, and restore a single box — the current-box.

G-015 — box-keeper
A bash script. On invocation it first locates the root-box to work from, using $PWD:

If $PWD starts with /dep, or there is no boxInfo in $PWD at all → the root-box is / (real-root-box).
Otherwise ($PWD does not start with /dep and a boxInfo exists there): read that boxInfo's first line (its box-ID). If the name of the parent (..) folder does not equal that box's version, $PWD doesn't fit the standard dep/.../${version}/ nesting shape — so $PWD itself is the virtual-root-box. If the parent folder's name does equal the version (meaning $PWD looks like it's nested inside a normal dependency tree), box-keeper moves up to that box's foster-parent and repeats the check, continuing until it finds the virtual-root-box.
Once the root-box is located, it: 3. Walks every boxInfo file one by one following the logical hierarchy (the real-parent/child-box relations declared inside boxInfo files — not the physical/real folder layout), building a table of that logical hierarchy called the kardex. 4. If it finds any mismatch or missing property, it has box-maker fix it (this phase is box-keeping). 5. Once existing boxes in the logical hierarchy are fixed, it scans the real hierarchy (actual folders on disk) for boxes absent from the kardex — orphans — and deletes them (this phase is box-burning).

G-016 — box-keeping
The reconciliation phase of running box-keeper: traversing the logical hierarchy, building the kardex, and having box-maker fix any box found mismatched or missing. (box-keeper performs both box-keeping and box-burning, in that order.)

G-017 — box-making
One of box-maker's jobs: fetching files from repositories, placing them into a box, foster-homing it, and editing boxInfo files. (Distinct from boxing and unboxing, below — box-making is what happens when there's no existing backup to draw on.)

G-018 — boxing
The process of making a backup of a box (archiving it to .box.tar.xz in box-rack).

G-019 — unboxing
The process of restoring a backup file — this happens when a boxInfo dependency row is satisfied by an existing backup file rather than needing a fresh fetch.

G-020 — box-sending
Sending a log file and one or more .tar.xz archives from a sender to a receiver. The receiver places the .tar.xz file(s) into /boxRack and the log file(s) into /boxMaker/logs, then edits the boxInfo where they want that archive used, adding a dependency row with the archive's box-ID. Calling box-maker on that box then triggers unboxing: box-maker finds the log and archive, foster-homes the new box, makes its folder, extracts (tar xvf) the archive into it, and sets the path/description fields on that dependency row in the real-parent's boxInfo.

(/boxMaker is the general home for all of box-maker's and box-keeper's own files — /boxMaker/logs is one folder within it.)

G-021 — box-burning
The process, run by box-keeper after box-keeping, of finding orphan-boxes (present in the real/physical hierarchy but absent from the kardex) and deleting them.

G-022 — box-rack
The single flat folder at the system root (/boxRack) where every box's archive (.box.tar.xz) is stored — package-boxes and etc-boxes alike — with no subfolders; uniqueness comes entirely from the filename.

G-023 — kardex
A temporary table built by box-keeper, representing the relations of all boxInfo files across the logical hierarchy. Used for both foster-homing and box-burning.
