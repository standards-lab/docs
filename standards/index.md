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
library; each draws its own dependency line, declares it, and implements the architecture whose
posture it satisfies — a framework-heavy standard implements its own architecture, not
Elemental. A standard that re-expresses another on a different technology declares that lineage
in its definition.

This index lists the blueprint's own standards, documented here in depth. Implementations that
live elsewhere — graduated organizations and external implementations of these architectures —
are referenced from the organization's catalog rather than documented here.

- [Go Elemental](go-elemental/index.md) — the Go implementation of the Elemental Architecture:
  a complete reference architecture on the Go standard library.
