---
key: topology-and-naming
name: Topology and naming
type: principle
level: go-minimal
---

# Topology and naming

How Go Minimal repositories, modules, packages, and release tags are named, and the module
layout of each tier. The tiers themselves are defined by the organizational
[repository topology](../../../principles/repository-topology.md).

## Repository names

A repository is named for its language and its tier, so the name states what a reader will find
and where it belongs. The language prefix is the only part a re-expression in another language
changes.

- **Core SDK** — `go-core`.
- **Application SDKs** — `go-<application>-sdk`: `go-web-sdk`. The `-sdk` suffix marks a
  development kit and appears on no other tier.
- **Infrastructure libraries** — `go-<technology>`, named for the technology the library
  presents as a service: `go-database`; `go-auth`, `go-storage`, `go-observability`,
  `go-messaging`, `go-ai` as each arrives. A provider sub-module is a nested directory named for
  the target API or system, never the driver it wraps: `postgres`, not `pgx`.
- **Templates** — the application SDK's name with `-template`: `go-web-sdk-template`.
- **Reference architectures** — named for the application built on the SDK: `go-web-service`.

## Module layout per tier

- A core SDK or application SDK repository is one Go module rooted at the repository, with no
  sub-modules. Packages keep their own short names (`config`, `lifecycle`, `logging`; `web`,
  `middleware`), so the import path names the repository and the identifier in code stays the
  package's: `web.Server`, `database.New`.
- An infrastructure library is one base module rooted at the repository, presenting the standard
  tier, with each provider a nested sub-module that has its own `go.mod`. Keeping the base and
  its providers in one repository makes a change spanning them atomic: it is made and exercised
  together through `go.work` before any tag is cut.
- A template repository roots its module at `template/`, so generation copies exactly the
  subtree and never the repository's management layer.
- A reference architecture is a single module and the repository's only releasable artifact.
- A package is split from its parent by growth or by dependency weight, never by topic alone: a
  sub-package is earned when its contents are a growth area or when a dependency is heavy enough
  that the rest of the module should not compile it.

## Release tags

- A module rooted at the repository is tagged `v<semver>`.
- A nested sub-module is tagged with its directory as prefix: `postgres/v0.1.0`,
  `template/v0.1.0`. The prefix is a resolution requirement: the Go module proxy resolves a
  subdirectory module's versions from tags prefixed with exactly that subdirectory.
