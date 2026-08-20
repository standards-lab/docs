# docs

The Standards Lab organization's documentation landing zone: the canonical home for its
architectures, standards, principles, and the documented details of every repository in the
effort. Managed with the marathon workflow; start from `context/README.md`.

## A context project

This repository is a marathon **context** project (`.claude/marathon.toml` declares
`kind = "context"`): there is no production source code — only documentation and the working
context that maintains it. The agent authors all of it directly, under the developer's review.

## The workspace docs tier

This repository is the single human-facing docs tier for the `~/architecture` workspace. Member
repositories author no `docs/` directory of their own: their settled design knowledge is
documented here, and their `context/` and `README.md` link to the pages that document them. A
repository that tightens a documented principle states that enhancement beside its link, in the
repository — a lower level enhances the principle it derives from and never loosens it.

The pages describe what exists. Marathon's decay rule does not apply to them — a page restating
the code is doing its job — but a page the code has moved out from under is a defect, fixed in
the change that moved the code or in the next session here.

Code on a page is illustrative only: it links to a direct example, or it encodes a generic
representation of the pattern that stands on its own; it never depends on the current source of
an external project.

## Structure

The hierarchy runs from universal to specific, and each directory level documents one level of
it:

- `index.md` — orientation: the effort, the vocabulary, and the map of everything here.
- `principles/` — organizational principles, universal across the organization.
- `architectures/` — the language-agnostic software architectures.
- `standards/<key>/` — each standard's definition, its principles, and its member repositories'
  documented details.

Every page opens with YAML front matter; the schema is documented in `README.md`. Prose follows
the marathon voice standard.
