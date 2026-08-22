---
key: health
name: Health
type: page
repo: go-web-sdk
---

# Health

The design of the liveness and readiness probes. The code and `doc.go` are authoritative for the
API; this page records the reasoning.

## The probes report; they do not check

`/healthz` reports that the process is up and serving HTTP and checks nothing else; that is
what makes an unanswered probe the liveness signal rather than a 500. `/readyz` aggregates the
readiness participants `RegisterHealth` builds from the lifecycle coordinator the composition
root supplies: the coordinator itself, first, under the fixed name `"lifecycle"`, then every
service that declares its own check, in start order — queried fresh on every request rather
than once when `RegisterHealth` is called, so a service the coordinator gains afterward still
appears on the next probe. Each participant is a go-core `lifecycle.Check`: the name lets an
operator read which subsystem is failing the probe, and a check with a nil checker reports not
ready, so a subsystem that failed to construct fails the probe rather than vanishing from it.
The pairing is defined in go-core because naming a readiness check is process-level, not
web-level: application-generic code declares its checks without importing this SDK.

## Readiness reflects the live state of the process

The readiness signal follows the lifecycle coordinator
([lifecycle and context ownership](../principles/lifecycle-and-context.md)) and each registered
participant, continuously:

- Not ready until every startup hook succeeds; a partially started process never reports ready.
- Not ready while any participant is degraded: a subsystem that loses its dependency (a dropped
  database connection, an unreachable upstream) fails the probe through its own check, and the
  process reports ready again once the subsystem restores.
- Not ready from the moment draining begins until the process exits. A draining process reports
  503 and stops receiving traffic before its shutdown hooks run; unlike degradation, draining is
  terminal, and readiness never returns.

`RegisterHealth` supplies the coordinator as the first participant itself, under the fixed name
`"lifecycle"`; each subsystem that reports its own readiness joins through the check on its
service declaration, and the coordinator returns them in start order. Exposing an in-progress
service's check this way is safe only because the transport itself registers at
`lifecycle.StageRoot`: no request reaches the probe until every numbered stage exists.
