---
key: overview
name: Standards Lab
type: index
---

# Standards Lab

Standards Lab is the blueprint for formalizing an effective, modern, agentic software
architecture strategy; the vision is stated in the
[organization profile](https://github.com/standards-lab). This landing zone documents the one
definitive architecture the blueprint builds out — the Elemental Architecture — and everything
beneath it. The blueprint is informative, not prescriptive: its arrangement may serve any
discipline that benefits from an agentic development workflow, and how an external effort
structures its own architectures and standards is its own to decide — this is a worked
example, not a schema.

Standardization here is emergent rather than decreed. A convention becomes a standard only
after working code has proven it: each module in the organization is a worked example that
others follow, and a pattern proven in one module is promoted upward — into the libraries
beneath it, then into its standard, and ultimately into the architecture's principles — as it
demonstrates that it generalizes.

## The hierarchy

Everything documented here sits at one of three levels of resolution, and each level below
belongs to the level above:

1. **Architecture** — [the Elemental Architecture](architecture.md): the compositional
   elements a program is built from and the rules that bind them, independent of language,
   with the [principles](principles/index.md) every level beneath satisfies.
2. **Standards** — technology-specific implementations of the architecture, each declaring its
   own dependency line and the principles its modules share.
3. **Modules** — the worked examples, in three classes: **library** (the core SDK, application
   SDKs, and infrastructure libraries), **template**, and **app**.

The narrowing rule binds the levels: a lower level may enhance — tighten — a principle it
derives from, and never loosen it. A module's principles satisfy its standard's; a standard's
satisfy the architecture's.

## Standards

- [Go Elemental](standards/go-elemental/index.md) — the Go implementation of the Elemental
  Architecture: a complete reference architecture on the Go standard library, with the smallest
  deliberate dependency surface. Its modules, by tier:
  - **Core SDK** — [go-core](standards/go-elemental/go-core/index.md): defines the common
    primitives useful across all Go Elemental application types.
  - **Infrastructure libraries** — [go-database](standards/go-elemental/go-database/index.md): the
    SQL service in two tiers, with providers as sub-modules.
  - **Application SDKs** — [go-web-sdk](standards/go-elemental/go-web-sdk/index.md): the SDK for
    building Go Elemental web service applications.
  - **Templates** — [go-web-sdk-template](standards/go-elemental/go-web-sdk-template/index.md):
    scaffolds an initial Go Elemental web service application.
  - **Reference architectures** — go-web-service: the holistic Go Elemental web service
    reference, grown in documented layers; versionless until its 1.0.

## Harness

The harness stands outside the hierarchy: it governs the agentic infrastructure the
organization builds its modules with, not the software they contain. Its principles are
cataloged at [`harness/`](harness/index.md), with the harness repositories as the worked
examples.

## Catalog

Implementations that live outside this blueprint — graduated organizations and external
implementations of its architecture — are referenced here rather than documented here. No
entries exist yet; the first arrives when the Elemental Architecture completes and graduates
to its own organization.
