---
key: schema-driven-artifacts
name: Schema-driven skill artifacts
type: principle
level: harness
---

# Schema-driven skill artifacts

A skill's configuration and state live in declared, schema-stable files inside the repository
they describe, so the repository remains the record of its own process. The skill documents each
artifact's canonical layout in its own files, reads the artifact at defined moments, and treats
anything projected outside the repository as a read-only mirror. Configuration travels with a
clone; nothing about the process lives only in a platform surface or a conversation.

## Worked examples

- [`marathon.toml`](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon/skills/marathon/mechanics/configuration.md)
  — a repository's marathon configuration: project kind, remote, workspace declaration, enabled
  extensions. Written once at `init`, read by every session.
- [`roadmap.toml`](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon-roadmap/skills/marathon-roadmap/references/manifest.md)
  — the marathon-roadmap extension's manifest of goals, tasks, and backlog, kept current by the
  sessions through the extension hooks.
