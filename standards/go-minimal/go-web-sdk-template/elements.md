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

- **Application** — what a generated service is: the deployable unit, here a web service. The
  template's `cmd/server` is its
  [composition root](../../../architectures/elemental-architecture/composition-root.md), and the
  composition root only declares: the three manifests name the service's infrastructure
  services, middleware, and domain-service modules.
- **Application layer** — `internal/app`. It owns the infrastructure registry and the process
  lifecycle, assembles the transport, and runs the process. The machinery lives here so the
  manifests stay declarative.
- **Infrastructure Services** — the entries in the infrastructure manifest, stored in the
  type-keyed registry. The baseline registers the logger; a database pool, storage, or auth
  client lands as one more entry.
- **Domain Service** — arrives with a generated service rather than the template. Its module is
  the unit of registration in the route manifest, and the baseline's empty manifest is where it
  lands.
- **Entity** — arrives with the domain services; the template reserves no vocabulary below it.
