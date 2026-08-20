---
key: go-database
name: go-database
type: repository
tier: infrastructure
repo: https://github.com/standards-lab/go-database
standard: go-minimal
---

# go-database

The SQL infrastructure library of [Go Minimal](../index.md): the database service in two tiers.
A reader who opens it sees the organization's SQL data-access conventions and nothing else, and
a consumer who depends on it pulls in exactly one service. The approach is plain SQL over
`database/sql` with a raw driver in the provider; no ORM, per the standard's
[dependency line](../principles/dependencies.md).

The repository is one base module, `github.com/standards-lab/go-database`, defining the standard
tier, with the `postgres` provider as a nested sub-module that pins its driver. The base module
depends on the standard library and go-core.

## Packages and modules

- **database** — the dialect-neutral core: the wrapper over a provider-constructed pool, meeting
  go-core's lifecycle contracts for startup, shutdown, and readiness; the `Dialect` interface
  providers implement; the configuration block that joins the layered load; and the error
  sentinels in dual-wrapped form. See [Service tiers in SQL](tiers.md) and
  [The dialect interface](dialect.md).
- **seed** — reference-data loading: a runner over the consumer's seed file system, a typed load
  function per table, one transaction per step, the decode format selected by extension.
- **postgres** — the PostgreSQL provider: constructs the pool over pgx's `database/sql` adapter
  and supplies the postgres dialect. See [Providers](providers.md).

The code and each package's `doc.go` are authoritative for the API; these pages document the
design.
