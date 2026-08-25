# The SQL meta language

Captured 2026-08-25, during the go-database `query-vocabulary` session that designed the query
package's composition core. Unscheduled R&D beyond the v1 path; `backlog.sql-meta-language` in
the workspace roadmap cites this concept.

## The gap

SQL never got the infrastructure of a modern programming language: no modules, no composable
definitions, no schema-aware editor support, no story for coexisting with the host language
that executes it. Every tool that touches the gap covers a corner. sqlc proves schema-typed
codegen but binds to one host language and composes nothing. PRQL and Malloy prove
compile-to-dialect-SQL but abandon SQL's own surface, carry no schema contract, and offer no
host bindings. dbt proves composable templates as untyped string macros. LINQ and Entity
Framework prove host-integrated querying by locking the query language inside one host. Rust's
sqlx proves compile-time checking of plain SQL — by asking a live database, for one host
language. sqlglot proves dialect compilation is tractable, transpiling across some twenty
engines; SQLMesh, built on it, is the closest thing to composable typed definitions in SQL's
own surface, and it targets analytics pipelines with no host bindings at all. ORMs as a
category paper over the gap by hiding SQL rather than composing it.

Nobody has combined: a SQL-compliant surface, schema-typed checking, composable definitions,
language-neutral host bindings, and an explicit standard/native tier split. The last item is
the organization's own contribution — the [service tiers](../../principles/service-tiers.md)
principle as language design — and it is what the existing attempts most conspicuously lack.

## The idea

A provider-agnostic SQL meta language: authored in its own files, type-checked against the
schema, compiled to provider-specific SQL, and called from any host language through generated
typed bindings.

- **The language.** A standard-SQL-compliant core — ISO/IEC 9075 semantics, not a reinvented
  query surface — plus what SQL never got: modules, parameterized composable definitions
  (fragments, functions, templates), and explicit imports of provider capabilities. The tier
  split becomes compiler policy: the core compiles for every provider; a unit that reaches for
  a native feature declares the reach, so "what breaks on a port" is a compiler answer instead
  of an audit.
- **Schema awareness.** The compiler consumes the schema — from the migration set, which the
  architecture already treats as the schema's owner, or from live introspection — and checks
  expressions against it: columns, types, nullability, relations, through a core type system
  with per-provider mappings. This is what makes editor completion real rather than keyword
  lists.
- **Compilation.** Typed AST, then provider backends emitting dialect SQL. The go-database
  query package's statement-as-value AST is this compiler's runtime shape seen from the other
  side: the Go composition core is the dynamic half, and the meta language would be the
  authoring surface above it, lowering to the same statements.
- **Host-language neutrality.** The protobuf model: the compiler emits provider SQL plus a
  neutral binding manifest — parameters in, row shapes out, as a JSON intermediate
  representation — and thin per-language generators produce typed wrappers: Go structs today,
  C# for the .NET mirror later. Hosts bind to generated types and the IR, never to the
  language. Distributing the compiler as WASM or a C ABI lets any toolchain embed it; a small
  optional runtime handles dynamic composition, which is the directives use case the query
  package serves today.
- **Tooling.** An LSP server for schema-aware completion and type errors, and a formatter.
  The LSP is the bulk of "feels like a first-class language."

## What it would take

A real language project: grammar, type checker, one backend (Postgres), one binding generator
(Go), and the LSP. A credible seed is a few focused milestones, not a weekend, and each
component has proven precedent — the combination is the novelty. The reference architecture is
the natural proving ground: its migrations own a real schema, its query package defines the
runtime AST a compiler would target, and its planned .NET mirror is the second host language
that keeps the bindings honest.

## The path out of concept stage

The goal case for this concept is a specification session: one session that converts this
document into a specification structured for autonomous loop engineering. The build divides
along a line the specification exists to draw — design decisions and verification oracles are
settled by the developer up front, and the component work (parser, inference, backends,
binding generators) runs as autonomous loops inside those frozen contracts, verified against
oracles the loops cannot edit: round-tripping through the formatter, differential execution
against a migrated engine, inferred row shapes checked against the engine's own
prepared-statement describe.

The specification is therefore a set of seam documents, each carrying a resolved or open
status: the surface grammar, the IR and binding-manifest contract, the type system and its
inference strategy, the oracle harness, and the milestone decomposition into loops. The
authoring session settles what one session can settle and leaves the rest explicitly open;
hard seams take their own follow-up sessions.

When the authoring session completes, the specification transitions out of this concept into
`context/spec/` in the meta language's own repository (the home named in the open questions).
Once every seam in that directory is resolved, it initializes the loop-engineering session
that builds the project. This concept page then decays to a pointer.

## Boundaries inherited from the architecture

- DML only. Migrations own the schema; DDL composition would compete with them. Metadata
  introspection stays provider-native (INFORMATION_SCHEMA is false-common: Oracle and SQLite
  diverge).
- The tier rule is load-bearing: the core never grows past the standard, and native reach is
  per-unit and declared — the language-level analogue of the import-boundary lint.
- Not a dependency of the v1 path. The query package stands on its own; the meta language is a
  possible authoring surface above it, and nothing in v1 waits on it.
- The split of labor is permanent, not transitional. Compiled units own what is authorable
  ahead of time; the host-side AST owns what the request shapes at runtime — dynamic filters,
  sorts, paging. The meta language displaces the static-query ladder (codegen, builders,
  templates), not the dynamic half the query package serves; that half is where ORMs live, and
  it stays a runtime concern by design.

## Open questions

- Surface syntax: strict ISO SQL plus a module/definition layer, or a conservative superset —
  and how parameters and fragments are declared in it.
- The IR contract: what the binding manifest guarantees across language generators, and how it
  versions.
- Schema source of truth: migration-set parsing versus introspection of a migrated database,
  and how provider type mappings are declared.
- Row-shape inference: checking a column against the schema is easy; typing a query's result —
  nullability through outer joins, aggregates over empty groups, CASE branches, set operations
  — is where sqlc has spent its defect budget and where sqlx punts to a live engine's describe.
  Owning the inference versus asking a migrated database is entangled with the schema-source
  question, because introspection gets the engine's own answer for free.
- Dynamic composition: where the compiled-unit boundary ends and the runtime (or the host-side
  AST, like the query package) takes over.
- Name and home: its own repository under the organization when it leaves concept stage.
