---
key: harness
name: Harness
type: index
---

# Harness

The harness level of the organization: the agentic infrastructure that codifies its development
processes, so the same processes apply consistently across every repository. The harness stands
outside the five repository tiers ([repository topology](../principles/repository-topology.md));
its principles govern how agent tooling is built, not what the software under development does.

The worked examples are the harness repositories themselves:
[claude-plugins](https://github.com/standards-lab/claude-plugins), the plugin marketplace whose
marathon workflow established most of these conventions, and claude-settings, the user-scoped
configuration repository.

## Principles

- [A skill is an index](skill-as-index.md) — a skill's entry file declares identity and routes
  into sub-layers; always-loaded conventions are pointers, referential material is links.
- [A standardized execution pipeline](execution-pipeline.md) — every command of a workflow skill
  runs one pipeline, and extensions integrate at universal hook points rather than inside
  commands.
- [Passive extension skills](passive-extension-skills.md) — a skill meant to be resolved by
  another skill's workflow never presents as a command.
- [Schema-driven skill artifacts](schema-driven-artifacts.md) — a skill's configuration and state
  live in declared, schema-stable files inside the repository they describe.
- [Configuration boundaries](configuration-boundaries.md) — user scope programs identity-level
  behavior; a skill programs only its own mechanics.

## Conventions not yet paged

Two further conventions are established in marathon and documented in its own files; they gain
pages here as they prove out beyond it:

- Workspace coordination — one orchestrated change across sibling repositories in dependency
  order ([marathon's design note](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon/skills/marathon/references/workspace-coordination.md)).
- The extension system — separately installed skills enabled per repository, acting only at the
  pipeline's hook points ([marathon's reference](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon/skills/marathon/references/extensions.md)).
