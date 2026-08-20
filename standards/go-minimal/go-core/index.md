---
key: go-core
name: go-core
type: repository
tier: core-sdk
repo: https://github.com/standards-lab/go-core
standard: go-minimal
---

# go-core

The Core SDK of [Go Minimal](../index.md): the common primitives useful across all Go Minimal
application types, and the first place the standard becomes code. A package is admitted only
when every application type in the standard uses it. Functionality specific to a single
application type belongs in that type's application SDK, and functionality specific to an
external technology belongs in that technology's infrastructure library; both depend on go-core.

The repository is a single Go module, `github.com/standards-lab/go-core`, with each primitive a
package inside it. It enhances the standard's [dependency line](../principles/dependencies.md)
to the standard library alone.

## Packages

- **config** — layered configuration: a base file, environment overlays, and secrets, resolved
  through a merge/finalize contract each subsystem's configuration implements. See
  [Configuration](config.md).
- **lifecycle** — the process lifecycle for long-running programs: concurrent startup, a
  readiness signal that tracks subsystem status through startup and drain, and timeout-bounded
  graceful shutdown. The conventions it fixes for every application are documented in the
  standard's [lifecycle and context ownership](../principles/lifecycle-and-context.md)
  principle.
- **logging** — the `*slog.Logger` a process writes through, built from a configuration that
  takes part in the layered load. See [Logging](logging.md).

The code and each package's `doc.go` are authoritative for the API; these pages document the
design.
