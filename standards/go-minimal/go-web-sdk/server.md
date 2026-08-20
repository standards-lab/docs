---
key: server
name: The server
type: page
repo: go-web-sdk
---

# The server

The design of the SDK's server bootstrap. The code and `doc.go` are authoritative for the API;
this page records the reasoning.

## The SDK owns the server bootstrap

The SDK wraps `http.Server` with the bootstrap every web service needs: configuration,
lifecycle integration, and an explicit separation between binding the listener and serving
requests. A service constructs the server from its configuration block and registers it with
the lifecycle coordinator; it never manages the listener or the serving goroutine itself.

## Bind on the calling goroutine, serve in the background

Starting the server is two distinct steps. The bind runs on the calling goroutine, bounded by
the context it receives, and a bind failure is a returned error. Serving begins in the
background only after the listener exists. Because a composition root registers the start method
as a lifecycle startup hook, and a startup hook's error fails the coordinator's startup
([lifecycle and context ownership](../principles/lifecycle-and-context.md)), a failed bind
stops startup before readiness ever reports ready: the process can never report healthy with
nothing listening. Binding first also makes the bound address knowable: a configured port 0
binds an ephemeral port and the server reports the assignment back, which is what lets tests
bind without racing a fixed port.

## A serve failure surfaces as a value

A failure after startup arrives on a buffered error channel the server exposes. The composition
root registers that channel as a monitored source, so the first failure ends the coordinator's
run and surfaces in the run's returned error. A server that logged the failure instead would be
choosing a policy the composition root owns. The expected end of a shutdown is not reported as a
failure; the channel closes when serving stops.

## Lifecycle wiring

The package registers no lifecycle hooks of its own and owns no shutdown timeout. Its start and
shutdown methods match the hook signatures of go-core's lifecycle coordinator, so the
composition root registers them as bare method values. The coordinator's drain context passes
directly to `http.Server.Shutdown`; a private timeout and a cancellation guard have no
counterpart here. Shutdown before a successful start is a no-op that leaves the server
startable, so the drain that follows a failed startup passes through cleanly; once it has
served, a server is single-use.
