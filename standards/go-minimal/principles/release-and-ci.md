---
key: release-and-ci
name: Releases and CI
type: principle
level: go-minimal
---

# Releases and CI

How Go Minimal repositories are versioned, released, built, and tested. Every repository
releases independently ([independent releases](../../../principles/independent-releases.md));
this page states the shared mechanics and the variants each tier adds.

## Releases

A releasable artifact is released by pushing its tag: `v<semver>` for a module rooted at the
repository, `<path>/v<semver>` for a nested sub-module (`postgres/v0.1.0`, `template/v0.1.0`).
Each artifact keeps its own `CHANGELOG.md` in Keep-a-Changelog form with dated headings
(`## [vX.Y.Z] - YYYY-MM-DD`); the release workflow derives the artifact from the tag and
extracts the matching changelog section into a GitHub release. There is no umbrella version
spanning a base module and its sub-modules.

Between releases, a prerelease tag (`v0.2.0-dev.1`) lets a consumer resolve an API still under
active development. A minor-or-above release purges them: its changelog entry states that it
includes every `-dev.N` change, and those tags are removed.

Every repository's first release is `v0.1.0`, its changelog opening with a snapshot of what the
artifact provides at that version.

## Branches and tags

Release preparation commits directly to `main`: the changelog date, the license, and README or
metadata edits. The tag is pushed only after main's CI run passes, so a release never points at
a commit that failed CI.

No release branch is retained. The tag is the durable artifact, and a branch can be recreated
from its tag at any time. A `release/v<major>.<minor>.x` branch is created only when a
maintenance line exists, meaning a patch to an older minor after `main` has moved past it, and
is kept only as long as that line is supported. Every other branch is deleted when it merges.

A published tag is never re-cut. Once the module proxy has fetched a version, the checksum
database pins that commit permanently, so moving the tag leaves consumers with a checksum
mismatch they cannot resolve. A failed release workflow does not affect module resolution,
since `go get` reads the tag rather than the GitHub release; the fix is to repair the workflow
on `main` and create the release for the existing tag.

## Coordinating a change across modules

Where a repository contains a base module and provider sub-modules, a committed root `go.work`
resolves them together during local development, so a change spanning them is built and tested
before anything is tagged. Pinned `require` versions are the committed steady state; a `replace`
directive is a transient bridge while a provider builds against unreleased base changes, and it
is removed once the base is tagged. The release ripple runs bottom-up: tag the base, bump the
provider's `require`, note it in the provider's changelog, tag the provider.

The tiers ripple the same way: a go-core change releases first and is taken up by coordinated
releases in the repositories above it, each pinning the versions it validated against.

## CI

Every repository runs `go vet`, `go test -race`, and golangci-lint on every push. The variants:

- A single-module repository runs the checks at the repository root.
- A multi-module repository runs a per-module matrix, each entry scoped to its module's
  directory; the matrix, the `go.work` use-list, and the task runner's module list name the
  same modules and are updated together.
- A template repository runs the checks inside its `template/` subtree.

The repositories are public: modules resolve through the public Go proxy and checksum database,
and CI needs no private-module configuration. The service-tiers import boundary is a
consumer-side check and runs in consumers; inside an infrastructure library, the module topology
already prevents the base from importing a provider.

## What ships in a module zip

A module zip includes every committed file below its `go.mod`, `context/`, `CLAUDE.md`, and
`.claude/` among them. Go provides no supported way to exclude them, and they cost nothing: the
toolchain compiles only imported `.go` files, so they ship as committed. The template is the
deliberate exception: its module boundary at `template/` exists so the management layer never
ships.

## Tasks

Each repository defines its developer tasks with mise (`build`, `test`, `vet`, `fmt`, `tidy`,
`lint`), each wrapping a plain Go command and looping over the module list where the repository
is multi-module. The repository works without mise; the tasks are a convenience.
