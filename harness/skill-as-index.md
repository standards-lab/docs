---
key: skill-as-index
name: A skill is an index
type: principle
level: harness
---

# A skill is an index

A skill's entry file (`SKILL.md`) carries three things: the YAML front matter that identifies and
triggers it, a description section that orients the reader, and an index into the skill's
sub-directories. Substantive content lives in the sub-layers, each named for the kind of knowledge
it holds; the entry file states each piece once, as an index entry, and never restates the detail.

The index distinguishes two loading modes:

- An `@path` pointer marks a critical convention that must be active whenever the skill is: the
  file loads with the entry file itself.
- A `./path` link marks referential material: consulted when its subject is in play, at the
  moment another file points to it.

The budget discipline follows from the modes. Every `@` pointer spends context in every session,
so it is reserved for the conventions the skill cannot operate without; everything else is a link.

## Sub-layers

The marathon skill names four, and the split generalizes:

- `behavior/` — always-active conduct, `@`-loaded.
- `mechanics/` — execution specs the skill acts from: pipelines, schemas, canonical layouts. The
  load-bearing spec is `@`-loaded; the rest are linked where they apply.
- `commands/` — one playbook per command, loaded when that command runs.
- `references/` — deep-dives consulted when their subject is in play.

## Worked examples

- [marathon's SKILL.md](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon/skills/marathon/SKILL.md)
  indexes nine command playbooks, `@`-loads its planning conduct and session pipeline, and links
  its mechanics and references.
- [marathon-roadmap's SKILL.md](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon-roadmap/skills/marathon-roadmap/SKILL.md)
  applies the same layout to an extension: the hook map is `@`-loaded, the hook instructions and
  manifest format are linked.
- A user-scope `CLAUDE.md` is the same index one level up; see
  [configuration boundaries](configuration-boundaries.md).
