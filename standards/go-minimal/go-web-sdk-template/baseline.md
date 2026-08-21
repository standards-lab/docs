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
`internal/app`: its `App` structure orchestrates the root composition, assembled from the
service's build points — route registration, the middleware stack, and the infrastructure
constructor in `internal/infrastructure`. `cmd/server` is the entrypoint alone.

- **The entrypoint** (`cmd/server`) owns the signal-derived root context and the exit code,
  through a testable run function: configuration load, `app.New`, run. It never changes as the
  service grows.
- **The application layer** (`internal/app`) separates cold start from hot start. `New`
  creates the lifecycle coordinator, constructs the infrastructure, assembles the router from
  the registered routes and the middleware stack, mounts the probes, and declares the server
  as the coordinator's root-stage
  service, with no I/O; `Run` is the hot start plus shutdown, delegated to go-core's
  coordinator. The server occupies the root stage — started after every infrastructure stage,
  drained first — so in-flight requests complete before the infrastructure beneath them
  closes.
- **Route registration** (`internal/app/routes.go`) declares the modules the router mounts,
  each constructor drawing its dependencies from the `Infrastructure` fields. The baseline
  registers none; the file is the build point for domain services.
- **The middleware stack** (`internal/app/middleware.go`) declares the router-level
  middleware, outermost first; middleware scoped to one domain service belongs on its module.
- **The infrastructure constructor** (`internal/infrastructure`) builds the `Infrastructure`
  struct: one concrete field per service, constructed in dependency order, each service with a
  lifecycle declared on the coordinator where it is constructed — its stage, startup,
  shutdown, and readiness check in one `Add` call. Construction opens nothing; connectivity
  belongs to a service's Start hook. A field either exists or the build fails, so a wiring
  mistake surfaces at compile time, and roles sharing a type, such as a write pool and a read
  pool, are distinct fields. The struct stops at the composition layer: the composition root
  reads its fields, and domain packages receive their dependencies as constructor parameters.
- **The configuration root** (`internal/config`) composes the library configuration blocks with
  the service's own settings and loads them through the layered files under one environment
  prefix.

The readiness probe reads the lifecycle coordinator and every service's named check: not ready
until startup completes, and not ready again once draining begins.

## Principles

- **Minimal and stable.** The template scaffolds the initial baseline architecture rather than
  a framework to track. Service integrations stay out: infrastructure libraries define them,
  and the reference architecture documents how one is integrated. What the template owns is the
  composition pattern: the App primitive, the build points, and the `Infrastructure` struct
  that receives each new infrastructure service.
- **Engine-free.** The baseline declares no data engine and depends on no provider. A generated
  service selects its providers in its own composition root, and that root, never the template,
  is where a provider is imported.
- **Generatable.** Generation copies the module and rewrites its path. Everything in the subtree
  must survive that rewrite: the module path is the only identity, and the copy is a running
  service from the first build.
- **Distributed as a module.** The template is a real, versioned module resolved through the
  public Go proxy, so generation works anywhere Go does, with no dependency on a hosting
  platform's template feature and symmetric with the scaffolding tools of other ecosystems.
