---
key: configuration-boundaries
name: Configuration boundaries
type: principle
level: harness
---

# Configuration boundaries

Agent programming divides by scope. User-scoped configuration programs identity-level behavior —
how the agent communicates, how it uses its everyday tools — for every session on the machine. A
skill programs the mechanics of its own workflow and nothing wider: it neither restates
identity-level behavior nor depends on any particular user-scope choice being present. Each
concern then has exactly one home, and neither drifts against the other.

The user scope, `~/.claude`, is kept in a source-controlled repository deployed into place by
symlink, with its `CLAUDE.md` the same index shape a skill uses ([a skill is an index](skill-as-index.md)): `@` pointers load
the always-active behavior files with every session, and plain references route to per-tool notes
consulted when the tool is in play.

## Worked examples

- `~/.claude` — the user-scope side: a `behavior/` directory (communication voice) that loads
  with every session, and a `tools/` directory (git) consulted on demand;
  [claude-settings](https://github.com/JaimeStill/claude-settings) is an example of the scope
  under source control.
- [marathon](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon/skills/marathon/SKILL.md)
  — the skill side: its `behavior/` tier holds only workflow conduct (planning discipline), and
  the skill makes no reference to communication style at all.
