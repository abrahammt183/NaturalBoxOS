# G-014: Boxing

**Term:** Boxing

**Definition:**
Boxing is the process of creating a Box archive from an existing Box.

A Boxer uses the Box's `boxInfo` as the source of truth and produces an archive containing the Box's files and metadata.

## Archive Naming

The archive name depends on the Box type.

### etc-Box

An etc-Box archive is named:

```text
${link}_${author}_${name}_${version}.box.tar.xz
```

Here, `${link}` is the name of the application that uses the configuration.

For example, an etc-Box containing Vim configuration named `pyVimPlus`, made by `bob`, at version `2.0.2`, is archived as:

```text
vim_bob_pyVimPlus_2.0.2.box.tar.xz
```

### app-Box

An app-Box archive is named:

```text
${type}_${author}_${name}_${version}.box.tar.xz
```

For example, Vim itself, an `x86_64_arch` package made by `arch`, named `vim`, at version `9.2.3`, is archived as:

```text
x86_64_arch_vim_9.2.3.box.tar.xz
```

## Purpose

Boxing preserves a Box as a portable archive that can be stored, transferred, or used for later unboxing.

## Related Terms

- **BoxMaker** — Creates or rebuilds a Box.
- **BoxKeeper** — Maintains Box relationships and consistency.
- **Unboxing** — Restores a Box from an archive.
- **BoxSending** — Transfers a Box archive.
- **Box archive** — The compressed result of Boxing.

## Important Distinction

Boxing is not the same as rebuilding a Box. Rebuilding creates or repairs the working Box; Boxing creates an archive of that Box.
