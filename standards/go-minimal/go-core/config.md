---
key: config
name: Configuration
type: page
repo: go-core
---

# Configuration

How a Go Minimal application's configuration is loaded and finalized. The `config` package
defines the mechanism; the code and its `doc.go` are authoritative for the API.

## A loader in the Core SDK

Without a shared loader, configuration decays into a per-package convention: each subsystem
defines a config type with merge and finalize behavior, and every consumer hand-writes the same
file-layering load. The `config` package makes the loader a base primitive: a single generic
`Load` every consumer calls, parameterized by the filenames and the environment-selector
variable. Consumers keep owning their config types and merge/finalize behavior; what the Core
SDK owns is the orchestration they all shared.

## The contract

A configuration is a type whose pointer implements two methods, expressed as a generic
constraint so each consumer writes them concretely against its own type:

- **Merge** overlays a source's set fields onto the receiver: a non-zero source field wins, a
  zero one is left alone, and nested sub-configurations delegate to their own merge. It is
  written field by field, without reflection. Because a set field always wins over an unset
  receiver, merging a fully populated layer onto a zero value reproduces that layer, so the base
  file loads the same way as every overlay.
- **Finalize** runs once, after every file is merged, in a fixed order: compose the
  environment-override names from the prefix it receives, apply defaults, read the overrides,
  then validate. Deferring validation to the end lets a required value arrive from any layer or
  from the environment. A malformed environment value fails Finalize: a bad input stops startup
  rather than being silently discarded.

## Layered load

`Load` reads up to four files from a directory, in precedence order, merging each that exists:

1. the base file — `config.json`
2. the environment overlay — `config.<env>.json`
3. the secrets file — `secrets.json`
4. the secrets overlay — `secrets.<env>.json`

then finalizes once. The active environment is the value of the selector variable; when it
resolves empty, both overlays are skipped. Every file is optional: a deployment can run on the
base file and environment variables alone, or on environment variables only. Later files win
over earlier ones, and the environment overrides read in Finalize win over every file.

## Environment-variable names

Environment overrides are declared, never read through scattered `os.Getenv` calls. A subsystem
pairs its configuration with an `Env` struct whose fields name the variables its Finalize reads,
composed from the prefix Finalize receives. Passing the prefix as a parameter makes the compiler
enforce the pairing between name composition and the finalize call; the earlier convention of
seeding the names before calling finalize silently disabled every override when the seed was
forgotten. An empty prefix composes no names, which is the hermetic form tests use.

## Configuration is ephemeral

A configuration exists only to initialize subsystems. A composition root loads it, constructs
subsystems from its values, and discards it; runtime code keeps the values it needs, not the
configuration. This fixes the point at which a setting is read at startup, and lets a subsystem
be constructed in a test from plain values without assembling a whole configuration graph.
