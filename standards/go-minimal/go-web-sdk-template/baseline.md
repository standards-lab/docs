---
key: baseline
name: The baseline
type: page
repo: go-web-sdk-template
---

# The baseline

The architecture the template scaffolds, and the principles that keep it minimal.

## Architecture

A single Go module built on go-core and go-web-sdk at pinned releases. The
[composition root](../../../architectures/elemental-architecture/composition-root.md) is
`cmd/server`: the entrypoint plus three manifests that declare the service's composition, one
concern per file. The machinery the manifests feed lives in `internal/`.

- **The entrypoint** owns the signal-derived root context and the exit code, and runs the phase
  sequence through a testable run function: configuration load, the infrastructure manifest,
  application assembly, run.
- **The infrastructure manifest** constructs every infrastructure service the application is
  composed on and registers each once, in dependency order; a registration declares the handle
  and the lifecycle integration (startup, shutdown, readiness) in the same call.
- **The route manifest** mounts the domain-service modules, each constructor drawing its
  dependencies from the registry, so a module's signature declares what its domain service uses.
  The baseline mounts none; the manifest is the build point.
- **The middleware manifest** stacks the router-level middleware, outermost first; middleware
  scoped to one domain service belongs on its module.
- **The application layer** (`internal/app`) separates cold start from hot start. Construction
  assembles the router from the wiring, mounts the probes, constructs the server, and binds the
  lifecycle, with no I/O; running is the hot start plus shutdown, delegated to go-core's
  coordinator. One composite hook drains the HTTP server before the infrastructure it depends
  on closes.
- **The infrastructure registry** (`internal/infrastructure`) is type-keyed: it stores one
  instance per type with its lifecycle declaration, retrieves by type, and panics on a wiring
  mistake at cold start. Startup runs in registration order, shutdown in reverse joining every
  error, and the registered checks feed the readiness probe. Retrieval stops at the composition
  layer: the manifests and the application layer read the registry, and domain packages receive
  their dependencies as constructor parameters. Roles sharing a type, such as a write pool and
  a read pool, register as defined wrapper types.
- **The configuration root** (`internal/config`) composes the library configuration blocks with
  the service's own settings and loads them through the layered files under one environment
  prefix.

The readiness probe reads the lifecycle coordinator and every registered check: not ready until
startup completes, and not ready again once draining begins.

## Principles

- **Minimal and stable.** The template scaffolds the initial baseline architecture rather than
  a framework to track. Service integrations stay out: infrastructure libraries define them,
  and the reference architecture documents how one is integrated. What the template owns is the
  composition pattern: the manifests, the application layer, and the registry a new
  infrastructure service lands in.
- **Engine-free.** The baseline declares no data engine and depends on no provider. A generated
  service selects its providers in its own composition root, and that root, never the template,
  is where a provider is imported.
- **Generatable.** Generation copies the module and rewrites its path. Everything in the subtree
  must survive that rewrite: the module path is the only identity, and the copy is a running
  service from the first build.
- **Distributed as a module.** The template is a real, versioned module resolved through the
  public Go proxy, so generation works anywhere Go does, with no dependency on a hosting
  platform's template feature and symmetric with the scaffolding tools of other ecosystems.
