---
key: overview
name: Standards Lab
type: index
---

# Standards Lab

Standards Lab is a clean-slate effort to establish standards for modern, inner-source enterprise
development. Modern is used specifically over "cloud native" and means the next generation of
primitives for what software emits — the library, the binary, the container image — not any one
deployment venue. The effort demonstrates a larger vision: interoperable, domain-driven
services; data composition over rigid, tightly coupled schema; deliberate control of the layers
above the transport; and an inner-source culture that standardizes reusable patterns across the
enterprise.

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

- [Go Minimal](standards/go-minimal/index.md) — the minimal-dependency Go standard: a complete
  implementation of the Elemental Architecture on the Go standard library, with the smallest
  deliberate dependency surface. Its member repositories, by tier:
  - **Core SDK** — [go-core](standards/go-minimal/go-core/index.md): defines the common
    primitives useful across all Go Minimal application types.
  - **Infrastructure libraries** — [go-database](standards/go-minimal/go-database/index.md): the
    SQL service in two tiers, with providers as sub-modules.
  - **Application SDKs** — [go-web-sdk](standards/go-minimal/go-web-sdk/index.md): the SDK for
    building Go Minimal web service applications.
  - **Templates** — [go-web-sdk-template](standards/go-minimal/go-web-sdk-template/index.md):
    scaffolds an initial Go Minimal web service application.
  - **Reference architectures** — go-web-service: serves as a holistic Go Minimal web service
    reference architecture (planned).
