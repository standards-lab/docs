---
key: logging
name: Logging
type: page
repo: go-core
---

# Logging

The design of the `logging` package. The code and its `doc.go` are authoritative for the API;
this page records the reasoning.

## The standard library owns the vocabulary

Hand-rolling a log-level type — a string type, a validity switch over four constants, a
conversion switch to the platform's level type — re-implements what `slog` already provides:
`slog.Level` parses names case-insensitively and accepts offsets such as `warn+2`. So `logging`
defines no level vocabulary. Converting the configured level is a delegation to `slog`, and
validation is that delegation returning no error. Adding a level, or accepting a spelling `slog`
accepts, costs the package nothing. The format field stays hand-written because the handler
choice belongs to this package and `slog` has no opinion about it.

## Level is a string because slog.Level's zero value is info

The one place the standard library's type is not used directly is the configuration field.
`slog.LevelInfo` is `0`, so a `slog.Level` field cannot distinguish "set to info" from "unset",
and the [configuration merge contract](config.md) runs on exactly that distinction. A layer
setting the level to info would be silently ignored. The level field is therefore a string whose
empty value means unset. The alternative, a pointer to `slog.Level` with nil as unset, was
rejected for putting a pointer field in an otherwise value-typed configuration and for moving
the rejection of a bad value from Finalize to JSON parsing.

## The writer is a constructor parameter

A configuration is values that are discarded after startup, so the log destination is an
argument to the constructor rather than a field. The composition root passes standard output; a
test passes a buffer, which is also what makes the package testable on its output rather than on
the handler's concrete type.

The constructor returns no error. Finalize is the validation point, and a caller that built a
configuration literal and skipped it falls back to info: a misconfiguration costs the process a
level, not its logs.

## The package constructs a logger; it does not retain one

`logging` is transport-agnostic. It defines no transport's request logger: a middleware belongs
to the transport that consumes it, so a command-line program or a worker that wants a
`*slog.Logger` does not compile `net/http` to get one. Subsystems take a `*slog.Logger` as an
ordinary dependency; only the composition root imports `logging`.
