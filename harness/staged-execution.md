---
key: staged-execution
name: Staged execution
type: principle
level: harness
---

# Staged execution

An agent implements an approved plan in stages, and the human reviews each stage before the next
begins. A stage is the smallest change set the toolchain can verify on its own — a compilation
unit brought to green with its tests and comments included — committed as its own revision, and
reported conversationally: the diff summary, the check result, and prose only on the decisions
the plan didn't spell out. The stage sequence runs in dependency order, so the whole is allowed
to be broken between stages while every broken consumer is a later stage.

The stage list is the plan's artifact, approved before any change is made. Staging is what makes
the review gate work: each review is small enough to take in fully, so it spends the human's
attention on judgment rather than transcription, and a wrong decision is caught at the stage that
makes it — before the stages built on top of it. Because a stage carries its own tests and
documentation, the work that is valuable but easy to skip lands with the change it belongs to
instead of being deferred to the end.

Review resolves one of three ways: approve (the next stage begins), adjust (the stage is edited
in place, re-verified, and re-reported), or re-plan (the remaining stages are re-planned, with
any invalidated commits reverted first, and the revised list approved like the original). Full
validation — the whole system's build and test run, plus a behavior check — runs once, after the
last stage.

## Worked example

marathon's `start` on a `code` project is
[`references/staged-execution.md`](https://github.com/standards-lab/claude-plugins/blob/main/plugins/marathon/skills/marathon/references/staged-execution.md):
the stage list is settled in the session's planning phase, each stage commits with its decision
lines, and closeout follows validation.
