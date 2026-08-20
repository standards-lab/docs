---
key: dependencies
name: Dependencies
type: principle
level: go-minimal
---

# Dependencies

Go Minimal's dependency line: the standard library first, and at most packages as idiomatic and
stable as the standard library itself (`golang.org/x/…`, `google/uuid`, and the like). No
frameworks. Vendor SDKs and database drivers enter only through provider sub-modules that pin
them, so importing a base module compiles no vendor code.

This line bounds dependency weight. Which packages may import a provider at all is the separate
import-boundary rule of the [service tiers](../../../principles/service-tiers.md) principle.

## The line per tier

- **Core SDK** — go-core enhances the line to the standard library alone. It is the bottom of
  the dependency graph; importing one of its packages pulls nothing beyond the standard
  library.
- **Infrastructure libraries** — a base module depends on the standard library and go-core.
  Each provider is a module of its own whose `go.mod` pins its driver, so a consumer that needs
  only the standard tier never pulls a driver.
- **Application SDKs** — the standard library and go-core. An application SDK has no providers:
  its standard tier is the technology's common standard over the transport the platform already
  provides, so there is no vendor library to isolate.
- **Templates** — go-core and the template's one application SDK, at pinned releases, and
  nothing else. A template is engine-free: no data engine declared, no provider imported; a
  generated application selects providers in its own composition root.
- **Reference architectures** — the application SDK, the infrastructure libraries it composes,
  and the providers of its declared stack, each at a pinned release. Provider imports are
  confined to the packages the import boundary declares.

## Integration is structural where it can be

A library integrates with go-core by implementing its contracts rather than by importing more
of it than it uses. The configuration contract is imported where a module's configuration block
joins the layered load. Lifecycle integration is structural: a module's start, shutdown, and
readiness methods match the coordinator's hook signatures, and the composition root registers
them, so the module itself needs no lifecycle import.
