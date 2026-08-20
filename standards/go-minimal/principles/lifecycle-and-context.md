---
key: lifecycle-and-context
name: Lifecycle and context ownership
type: principle
level: go-minimal
---

# Lifecycle and context ownership

Every Go Minimal application follows one process lifecycle, hosted by go-core's `lifecycle`
package, and one context-ownership convention. The lifecycle realizes the Elemental
Architecture's infrastructure-service contract in Go: ordered startup, reverse-order drain, and
readiness reported to the probes.

## The composition root owns the signal context

The composition root traps signals and derives the root context; it passes that context to the
lifecycle coordinator's blocking run call, the one call that owns the sequence. Subsystems are
registered as hooks and monitors while the coordinator waits; nothing executes before the run
begins, and the coordinator installs no signal handlers of its own.

## Cold start, hot start, drain

Cold start constructs every object from configuration with no I/O, so a construction mistake
fails before anything runs. Hot start brings the long-running subsystems up concurrently;
a startup failure drains what did start, and readiness never reports a partially started
process. The drain is two-phase and coordinator-driven: the run context is cancelled, then each
shutdown hook runs against a fresh timeout-bounded context, so cleanup is not pre-cancelled and
a hook needs no cancellation guard of its own.

A subsystem declares runtime failure through a monitored channel: the first non-nil error ends
the run.

## Readiness reflects the live state of the process

The coordinator is ready once every startup hook succeeds, and not ready again the moment
draining begins, so a readiness probe reports a draining process as unavailable before its
shutdown hooks run. A subsystem exposes its own readiness through the coordinator's checker
contract, and that state is live: a subsystem that degrades fails the probe until it restores,
and a process that begins draining stays not ready until it exits. Leaf components take a plain
`context.Context`, keeping them usable without the coordinator.
