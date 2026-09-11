# BoxKeeper

## 1. Definition

BoxKeeper is a Bash script responsible for maintaining the consistency of the Box system.

BoxKeeper does not make or repair Boxes itself. When a problem requires a Box to be made, rebuilt, or moved, BoxKeeper calls BoxMaker.

BoxKeeper has two modes:

- normal mode;
- cleanup mode.

Normal mode is the regular maintenance operation. Cleanup mode is an occasional, heavy maintenance operation.

## 2. Normal Mode

When the user invokes BoxKeeper in normal mode, BoxKeeper performs the following operations.

### 2.1 Locate the Relevant Box

BoxKeeper starts from the current working directory (`$PWD`) and searches for `boxInfo`.

If no `boxInfo` is found in the current directory, BoxKeeper continues through the chain of physical directory upward to find the topLevelBox.

If no relevant Box is found in the directory chain, BoxKeeper uses `/boxInfo` as the topLevelBox.

### 2.2 Locate the Top-Level Box

After locating the relevant Box, BoxKeeper follows the directory chain upward until it reaches the top-level Box.

The top-level Box is either:

- the Box defined by `/boxInfo`; or
- the root Box of a virtual development environment.

### 2.3 Index the Logical Tree

BoxKeeper reads the top-level Box's `boxInfo` recursively through its logical dependency tree.

It creates an index of all `boxInfo` files belonging to that logical tree.

The index is required for checking the relationships between Boxes and for maintaining foster-homing.

### 2.4 Check the Logical Tree

BoxKeeper uses the index to look for problems in the logical tree, including:

- missing Boxes;
- semidetached information;
- inconsistent parent relationships;
- other inconsistencies discovered during maintenance.

### 2.5 Maintain Foster-Homing

An OuterBox may have a chain of open-boxes between its realParentBox and fosterParentBox.

BoxKeeper checks the indexed `boxInfo` files for changes to `openBox` and `closeBox` states.

If a Box changes from open-box to close-box, affected OuterBoxes must be moved into that newly closed Box.

If a Box changes from close-box to open-box, affected OuterBoxes may need to move upward to the next appropriate closed Box.

When movement or another repair is required, BoxKeeper calls BoxMaker.

### 2.6 Repair Problems

Whenever BoxKeeper detects a problem that requires making, rebuilding, or moving a Box, it calls BoxMaker to perform the required operation.

## 3. Cleanup Mode

Cleanup mode is a separate mode intended for occasional use because scanning the physical tree is a heavy operation.

Cleanup mode performs the normal-mode operations first. It then performs additional orphan detection.

### 3.1 Scan the Physical Tree

BoxKeeper scans the physical tree to find Boxes that are not represented by valid relationships in the logical tree.

### 3.2 Identify Orphaned Boxes

An orphaned Box is a Box that:

- has no realParentBox;
- is not a top-levelBox;
- is not an etc-box.

Manually made Boxes are etc-boxes. Therefore, a manually made Box is not considered orphaned merely because it is not referenced by an app-box.

### 3.3 Remove Orphaned Boxes

After identifying orphaned Boxes, BoxKeeper removes them.

Cleanup mode must not remove:

- top-level Boxes;
- etc-boxes;
- Boxes that have a realParentBox.

## 4. Modes Summary

| Mode | Logical-tree index | Logical-tree maintenance | Physical-tree scan | Orphan removal |
|------|--------------------|--------------------------|---------------------|----------------|
| Normal | Yes | Yes | No | No |
| Cleanup | Yes | Yes | Yes | Yes |

## 5. Responsibility

| Component | Responsibility |
|-----------|----------------|
| BoxKeeper | Locate the relevant top-level Box, index the logical tree, detect inconsistencies, maintain foster-homing, and request repairs |
| BoxMaker | Make, rebuild, and move Boxes when requested by BoxKeeper |
| `make` | Provide the underlying build and dependency mechanism used by BoxMaker |
