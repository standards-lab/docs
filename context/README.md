# docs context

The documentation landing zone of the Standards Lab organization: one repository documenting the
organization's architectures, standards, principles, and the details of the repositories that
implement them, published as plain markdown a person reads on GitHub and a site serves without
rewriting.

This repository is the workspace's docs tier. In a marathon workspace, human-facing
documentation is centralized here rather than authored per repository: member repositories keep
their working `context/` and link to the pages that document them, stating any enhancements to
the derived principle beside the link.

## Capabilities

- **Orientation** (`index.md`) — the overview a reader starts from: the effort, the hierarchy,
  the vocabulary, and the routed map of the pages.
- **Organizational principles** (`principles/`) — the conventions universal across the
  organization, one page per principle.
- **Architectures** (`architectures/`) — the language-agnostic software architectures; Elemental
  Architecture is the first.
- **Standards** (`standards/`) — each standard's definition, its standard-level principles, and
  its member repositories' documented details; `go-minimal` is the first.
- **Metadata schema** — YAML front matter giving every page machine-readable identity, documented
  in `README.md`.

## Site hosting

Deferred. The settled direction: a thin `standards-lab.github.io` repository whose Actions
workflow checks this repository out, builds, and deploys with `actions/deploy-pages`, triggered
by `repository_dispatch` from here — so the apex `https://standards-lab.github.io/` serves this
content while this repository stays the sole authoring home. Theme and toolchain are open; the
pages are kept toolchain-neutral (plain markdown, YAML front matter, relative links) so the
choice stays free.
