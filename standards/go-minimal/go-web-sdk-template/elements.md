---
key: elements
name: The elements in the template
type: page
repo: go-web-sdk-template
---

# The elements in the template

How the template realizes the
[Elemental Architecture](../../../architectures/elemental-architecture/index.md)'s elements. The
architecture is defined once, at the organization level; this page maps its elements onto the
scaffolded baseline and adds nothing to the definition.

- **Application** — what a generated service is: the deployable unit, here a web service. Its
  [composition root](../../../architectures/elemental-architecture/composition-root.md) is
  `internal/app`, where the `App` structure orchestrates the root composition from the
  service's build points; `cmd/server` is the entrypoint alone.
- **Application layer** — `internal/app`. It owns the `Infrastructure` struct and the process
  lifecycle, assembles the transport, and runs the process — all inside `New` and `Run`, so
  the build points stay declarative.
- **Infrastructure Services** — the fields of `Infrastructure`, constructed by
  `infrastructure.New` with their lifecycles declared on the staged coordinator. The
  baseline's one field is the logger; a database pool, storage, or auth client is one more
  field with its lifecycle declaration.
- **Domain Service** — arrives with a generated service rather than the template. Its module is
  the unit route registration mounts, and the baseline's empty registration is where it enters
  the application.
- **Entity** — arrives with the domain services; the template reserves no vocabulary below it.
