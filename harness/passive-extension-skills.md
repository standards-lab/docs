---
key: passive-extension-skills
name: Passive extension skills
type: principle
level: harness
---

# Passive extension skills

A skill meant to be resolved by another skill's workflow never presents as a command.
`user-invocable: false` in the SKILL.md front matter keeps the skill out of the slash-command
menu while the model can still load it, so the host workflow resolves and applies it at the right
moment and the developer's command surface stays limited to the skills that are actually entry
points.

## Worked example

[marathon-roadmap](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon-roadmap/skills/marathon-roadmap/SKILL.md)
declares `user-invocable: false`: marathon sessions resolve it as an enabled extension and fire
its hooks, and it never appears beside `marathon` as a command of its own.
