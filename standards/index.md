---
key: standards
name: Standards
type: index
---

# Standards

A standard implements an architecture on a specific technology: it declares its goals, the
architecture its members implement, the dependency line defining which dependencies its modules
may declare, and the principles its member repositories share. A repository belongs to exactly
one standard, and a standard's principles enhance the organization's without loosening them.

An organization sustains parallel standards. A standard built on a web framework, with an SDK
and a template idiomatic to that framework, is as legitimate as one built on the standard
library; each draws its own dependency line and declares it. A standard that re-expresses
another on a different technology declares that lineage in its definition.

- [Go Minimal](go-minimal/index.md) — the minimal-dependency Go standard: a complete
  implementation of the Elemental Architecture on the Go standard library.
