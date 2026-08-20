---
key: minimal-footprint
name: A minimal, deliberate dependency footprint
type: principle
level: organization
---

# A minimal, deliberate dependency footprint

Every dependency is deliberately chosen. A dependency admits code the organization does not
author into what it ships. Its vulnerabilities become the program's vulnerabilities, and its
upgrade and deprecation cycles become technical debt on a schedule the organization does not
control. The bounding guideline: operate as close to the lowest level idiomatic standard
necessary to meet your requirements. Reach for the platform and its standard library first, and
adopt an established idiom above them only when a requirement demands it.

Heavy dependencies, such as vendor SDKs and database drivers, are isolated behind sub-modules
that present a light interface. A consumer that needs only the interface never compiles the
weight. [Service tiers](service-tiers.md) applies this to infrastructure libraries: the base
module defines the standard tier, and each provider sub-module pins its own vendor library.

Where the dependency line is drawn, meaning what a base module may take at all, is not
organizational. Each standard draws its own line as part of its definition, and a repository may
enhance it, tightening it further for its own scope and never loosening it. A standard built on
a web framework, with an SDK and a template idiomatic to that framework, is as legitimate as one
built on the standard library; each draws its line and declares it.
