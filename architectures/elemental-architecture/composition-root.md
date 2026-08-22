---
key: composition-root
name: The composition root
type: principle
level: elemental-architecture
---

# The composition root

Every application has exactly one composition root: the package where the application assembles
its dependencies. It constructs the infrastructure services, loads the configuration, wires the
transport, and declares how they compose. Nothing else in the application constructs a
dependency; everything else receives its dependencies from the composition root.

## It declares; it does not execute

The composition root's output is a declared composition, and execution belongs to the
[application layer](index.md). The root states which infrastructure services exist and in what
order they start, which domain-service modules mount on which routes, which Reactors run
alongside the transport, and which middleware wraps it. A root is not limited to one runner: the
transport and any Reactors all register on the same lifecycle coordinator, ordered by stage like
any other service. Keeping the root declarative keeps the application's entire composition
readable in one place: a reviewer can see everything the application is made of without tracing
execution.

## It is the boundary for provider imports

The composition root is where the [service tiers](../../principles/service-tiers.md) import
boundary anchors. Only the composition root, the application's binaries, and packages that
declare native use import a provider. The root constructs the provider and passes the resulting
service downward as an ordinary dependency, so every package below it stays provider-free and
works against the standard tier.

## It owns process entry and exit

The composition root owns the signal context and the exit code: it traps the process signals,
derives the root context, and passes that context to the run call that executes the declared
composition. Configuration is loaded here and discarded here; subsystems receive values, never
the configuration itself.

## It fails loudly, at startup

The composition root is written once and runs at boot, so every wiring mistake fails there,
immediately and loudly: a duplicate registration, a missing dependency, a malformed route
prefix. A loud failure at startup is cheap; the same mistake surfacing silently in production is
not. Implementations enforce this with panics at construction time, and a wiring mistake the
language can surface at compile time — a dependency that is a concrete field rather than a
looked-up entry — fails earlier still; the principle sets the latest acceptable moment, not the
preferred one.
