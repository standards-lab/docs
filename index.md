---
key: overview
name: Standards Lab
type: index
---

# Standards Lab

Standards Lab is the blueprint for formalizing an effective, modern, agentic software
architecture strategy; the vision is stated in the
[organization profile](https://github.com/standards-lab). This landing zone is the catalog: it
documents the blueprint's own architectures, standards, and principles in depth, and references
implementations that live elsewhere — graduated organizations and external implementations —
rather than hosting their documentation. An architecture or standard intended for production
use graduates to its own organization, whose name carries the standard's identity.

Standardization here is emergent rather than decreed. A convention becomes a standard only after
working code has proven it: each repository in the organization is a worked example that others
follow, and a pattern proven in one repository is promoted upward — into the libraries beneath
it, then into its standard, and ultimately into the organization's principles — as it
demonstrates that it generalizes.

## How the documentation is organized

Everything the organization defines sits at one of four levels, layered from universal to
specific, and each level below belongs to the level above:

1. **Organizational principles** — conventions universal across the organization.
2. **Architectures** — technology-agnostic definitions: the compositional elements of a domain
   and the rules that bind them.
3. **Standards** — technology-specific implementations of an architecture, each declaring the
   principles its member repositories share.
4. **Repositories** — the worked examples: each member repository's documented details, grouped
   under its standard by tier.

Three terms recur. An **architecture** defines a domain's elements and rules independent of any
technology. A **standard** implements an architecture on a specific technology. A **principle**
is a singular convention attached to any level. The narrowing rule binds the levels: a lower
level may enhance — tighten — a principle it derives from, and never loosen it. A repository's
principles satisfy its standard's; a standard's satisfy the organization's.

## Organizational principles

- [Resolution matches purpose](principles/resolution.md) — software interfaces with an external
  technology at the resolution its purpose requires, wrapping finer resolutions beneath coarser
  interfaces.
- [Dependencies flow downward only](principles/downward-dependencies.md) — and interfaces are
  defined where they are consumed, including across standards.
- [A minimal, deliberate dependency footprint](principles/minimal-footprint.md) — every
  dependency is chosen on purpose; each standard draws its own dependency line.
- [Service tiers](principles/service-tiers.md) — every service an infrastructure library offers
  presents two tiers: a standard tier usable across any provider of the technology, and the
  provider's own native tier, contained so it never spreads through a consumer.
- [Independent, artifact-keyed releases](principles/independent-releases.md) — every
  standardized repository releases on its own tags.
- [Repository topology](principles/repository-topology.md) — the tiers of repository the
  organization builds, what a repository of each tier contains, and the downward dependency
  direction between them.

## Architectures

- [Elemental Architecture](architectures/elemental-architecture/index.md) — an organizational
  application architecture: the compositional elements a program is built from — application,
  reactor, infrastructure services, domain services, entities — and the rules that bind them.

## Harness

The harness level stands outside the four documentation levels above: it governs the agentic
infrastructure the organization builds its repositories with, not the software they contain. Its
principles are cataloged at [`harness/`](harness/index.md), with the harness repositories as the
worked examples.

## Standards

- [Go Elemental](standards/go-elemental/index.md) — the Go implementation of the Elemental
  Architecture: a complete reference architecture on the Go standard library, with the smallest
  deliberate dependency surface. Its member repositories, by tier:
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
