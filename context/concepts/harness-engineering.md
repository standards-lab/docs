# Harness engineering reference

Captured 2026-08-25, during the claude-plugins `marathon-extensions` session. Unscheduled; a
candidate for the workspace roadmap when the docs tier takes its next step.

## The idea

Add a layer to the docs landing zone that catalogs principles and strategies for building agentic
harness infrastructure. The marathon skill is establishing conventions that other skill writers
could reuse, and today they live only inside marathon's own files; the landing zone would document
each convention as a reference page, alongside the pages that define the service tiers.

Candidate entries, from the conventions marathon is laying down:

- schema-driven skill artifacts (`marathon.toml`)
- workspace coordination
- an extension system with encapsulated external extensions
- a standardized execution pipeline with command and hook integrations
- the `~/claude-settings` tool and behavior programming

The list is a starting point, not the catalog; more conventions will surface as the plugins grow.

## Why it matters

The organization's reference architecture treats the harness as a level of its own. A skill writer
starting a new plugin should be able to read how these problems were solved once, rather than
reverse-engineering marathon. Cataloging the conventions also pressure-tests them: a principle that
can't be stated for a general reader probably isn't settled.

## Open questions

- Where the layer sits in the landing zone's existing structure, and what its pages are named.
- Which entries are principles (stable, prose) and which are worked examples pointing into
  marathon's files.
- How the catalog stays honest as the conventions evolve — likely the same review discipline the
  rest of the docs tier uses.
