---
key: objective-instructions
name: Objective instructions
type: principle
level: harness
---

# Objective instructions

A skill's instructions state what the system supports, in the present tense, and nothing else.
When a feature or concept is removed, the text that described it is removed with it — never
replaced by counter-instructions ("X is retired", "no longer do Y") that define the current
behavior by contrast with a past the reader cannot see. The changelog is the home for what
changed and what was removed; the instructions are the home for what is.

Counter-instructions are layered context at its worst: each one presumes the superseded concept,
so the reader must reconstruct the old model to understand the correction, and the corrections
accumulate with every revision. Instructions that only describe the present stay the same size as
the system they describe, and a fresh reader — human or agent — pays no tax for the history.

The same discipline applies to the human's part in a process: each instruction states what it
expects of the human at the moment it acts — an approval, a review, a confirmation — rather than
maintaining a standing role document that other instructions must stay consistent with.

## Worked example

marathon v0.9.0 replaced its implementation-guide workflow with staged execution. The skill's
files describe only the staged workflow; the removals are recorded in the
[CHANGELOG](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon/CHANGELOG.md),
and the prior workflow remains readable at the `marathon/v0.8.0` tag.
