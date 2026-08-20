---
key: tests-and-docs
name: Tests and documentation
type: principle
level: go-minimal
---

# Tests and documentation

## Tests: co-located, black-box, hermetic

Tests are `{file}_test.go` files co-located with the source they cover, in an external test
package (`package <pkg>_test`). They exercise only the public API; private infrastructure is
covered transitively through the public entry points that use it.

Tests are hermetic: `go test -race ./...` passes with no service running, so CI needs no
database container, no network fixture, and no fixed port. Each tier reaches hermeticity its
own way:

- **Infrastructure libraries** substitute the engine below the standard interface: a
  `database/sql/driver` stub whose connectivity a test breaks and restores, or a fake connector
  that counts transactions. Provider tests assert construction, which performs no I/O.
- **Application SDKs** exercise handlers through recorded requests with no listener; a test
  that must listen binds port 0 and reads the assigned port back, never a fixed port.
- **Templates and reference architectures** run their race suite against the composed baseline
  the same way, with the readiness probes as the observable surface.

A test helper stays in its test package until more than one test package needs it; then it is
hoisted into a module-private `internal/<pkg>test` package, following the standard library's
`httptest`/`fstest` naming. Nothing in it is API.

## doc.go and godoc

Production source is written without doc comments; the agent writes godoc. Each package has
exactly one `doc.go` containing only the package comment, and that comment is the authoritative
description of the package's API.
