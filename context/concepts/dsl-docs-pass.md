# The DSL docs pass — inventory

Captured at the 2026-08-31 workspace retrospective; `v1.data.sql.docs` in the coordinator's
roadmap cites this note as the session's work list. The strategy it documents is the
coordinator's `standards-lab/context/design/dsl-driven-services.md`. The note is deleted at
the pass's close.

## New pages

- **The DSL-driven-services principle** (go-minimal principles): the protocol-driven versus
  DSL-driven distinction, the five things the host layer exists for, sufficiency-not-only-
  capability, structural injection safety, portability by discipline with the trade stated —
  the strategy's §2.1–2.5, in page form.
- **The context-architecture principle** (org principles): the single-source-of-truth rule
  for written context, from the coordinator's `design/context-architecture.md`, stated for
  human contributors.
- **The go-web-service pages**: the service is listed as "(planned)" in `index.md` and
  `standards/go-minimal/index.md` despite six merged PRs, a migrated schema, and being the
  subject of the whole v1 goal tree; no `standards/go-minimal/go-web-service/` directory
  exists, and `tier: reference` — declared as a valid front-matter value in `README.md` — is
  used by zero pages. Settle the **service page type** here: the v1 criteria require "a
  `docs/` page names each capability the service consumes, its class, and the port list" — a
  per-capability page shape the current IA (principle pages + per-library design pages) has
  no slot for.

## go-database pages (the bounded invalidation)

- `standards/go-minimal/go-database/layers.md` — total loss: all 69 lines are the four-layer
  ontology. Replaced by the v0.4 shape.
- `standards/go-minimal/go-database/index.md` — "built as four layers" and three of six
  package bullets; rewrite around `database`/`query`/`migrate`/`seed`/`postgres`.
- `standards/go-minimal/go-database/dialect.md` — the "Render capabilities" half
  (`PagingRenderer` override, `ReturningRenderer` declared-native) presupposes Go-side
  rendering; rewrite around what survives (`Placeholder`, `MapError`, the divergence ledger's
  by-discipline form).
- `standards/go-minimal/go-database/tiers.md`, `providers.md` — survive; verify wording.
- `standards/go-minimal/go-web-sdk/reads.md:28` — "deliberately parallel to go-database's
  read vocabulary" and "go-database answers with its typed unknown-field error" (that error
  lived in `operation`; it survives into `query` — re-anchor, don't delete).
- `standards/go-minimal/go-web-sdk/middleware.md` — carries the incorrect claim that seeding
  the recorded status "removes any need to intercept the body write" (the shared writer must
  intercept `Write`); corrected by the `v1.web.adapter` session, verified here.

## Concept reframe

- `context/concepts/sql-meta-language.md` (this repo) — annotated at the retrospective,
  rewritten here: the `ast` package it treated as its permanent runtime half and lowering
  target retires; authored SQL files become the authoring surface it sits above, its
  schema-typing premise aligns with the `migrate` mechanism, and build-time fragment
  composition is its recognizable phase one. The "permanent split of labor" boundary is
  restated in those terms.

## Catalog and harness adjacents (verified here, owned elsewhere)

- `standards-lab/references.md` go-database and go-web-sdk entries refresh with the releases
  (owned by `backlog.workspace-sweep`; this pass verifies the go-database entry against the
  new pages).
- The harness tier has no page describing the marathon code-project loop's staged form after
  v0.9.0 — the marathon session's closeout updates it; this pass verifies consistency with
  the project-kinds paragraph.
- The roadmap's `backlog.validation-first` names its docs page as remaining work and lost its
  worked exhibit (the ast render layer); when that page is written, a current exhibit is
  named.
