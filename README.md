# Standards Lab

The documentation landing zone of Standards Lab — the blueprint organization
([github.com/standards-lab](https://github.com/standards-lab)): the canonical, discoverable
home for the Elemental Architecture, its standards, and the documented details of the modules
that implement them. Start reading at [`index.md`](index.md).

## The hierarchy

Documentation runs down three levels of resolution — architecture → standard → module — and
each directory level documents one:

| Level | Location | What it defines |
|-------|----------|-----------------|
| Overview | [`index.md`](index.md) | The blueprint, the hierarchy, its vocabulary, and the route to every page |
| Architecture | [`architecture.md`](architecture.md) | The Elemental Architecture: the elements a program is built from and the rules that bind them, with its [principles](principles/index.md) |
| Standards | [`standards/<key>/`](standards/index.md) | Each standard: a technology-specific implementation of the architecture, with its own principles |
| Modules | `standards/<key>/<module>/` | Each module's documented details, in three classes: library, template, app |
| Harness | [`harness/`](harness/index.md) | Principles for the agentic infrastructure the organization builds with; outside the hierarchy |

Three terms bind the levels. An **architecture** defines a domain's compositional elements and
the rules that bind them, independent of any technology. A **standard** implements an
architecture on a specific technology and declares the principles its modules share. A
**principle** is a singular convention attached to any level of the hierarchy. The narrowing
rule joins them: a lower level may enhance — tighten — a principle it derives from, and never
loosen it.

## Page metadata

Every page opens with YAML front matter. Common fields:

```yaml
key:  go-elemental        # stable identifier; matches the page's path segment
name: Go Elemental        # prose name
type: standard          # index | principle | architecture | standard | module | page
```

Per-type fields:

- `type: principle` — `level`: `architecture`, `harness`, or the key of the standard the
  principle attaches to.
- `type: architecture` — `status`: `draft` | `active` | `superseded`.
- `type: standard` — `architecture`: the key of the architecture the standard implements;
  `status` as above; `derives`: the key of the standard it re-expresses, when it does.
- `type: module` — `tier`: `core-sdk` | `infrastructure` | `sdk` | `template` | `reference`
  (the software refinement of the module classes library | template | app);
  `repo`: the repository's GitHub URL; `standard`: the key of its standard.
- `type: page` — `repo`: the key of the repository the page details.

## Conventions

- Pages are plain markdown with relative links, readable on GitHub as-is and toolchain-neutral
  for a documentation site.
- Code on a page is illustrative only: it links to a direct example, or it encodes a generic
  representation of the pattern that stands on its own.
- A page describes what exists; planned work is marked as planned.

## License

[Apache License 2.0](LICENSE).
