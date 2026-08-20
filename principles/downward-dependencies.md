---
key: downward-dependencies
name: Dependencies flow downward only
type: principle
level: organization
---

# Dependencies flow downward only

A repository depends only on repositories at lower tiers of the [repository
topology](repository-topology.md), and knowledge of a dependency runs the same direction: a
repository names its dependencies; no repository knows its dependents. Interfaces are defined
where they are consumed: a library declares the contract it needs from its dependencies, never
the contract a consumer might want from it, and a repository documents its dependencies without
naming its dependents.

## Across standards

The rule applies between standards the same way it applies between repositories. A repository
belongs to exactly one standard — the one whose author answers for it. Another standard that
finds a member's modules sufficient adopts them as ordinary dependencies at pinned releases,
never as members: the adopting standard authors only what is specific to itself, its dependency
line must admit what the adopted modules require, and its definition may note the adoption for
legibility, the same way a repository documents its dependencies. No second membership mechanism
exists.
