# Standards Lab

The documentation landing zone of Standards Lab: the canonical, discoverable home for the
organization's architectures, standards, principles, and the documented details of the
repositories that implement them. Start reading at [`index.md`](index.md).

## The hierarchy

Documentation is layered from universal to specific, and each directory level documents one
level of the hierarchy:

| Level | Location | What it defines |
|-------|----------|-----------------|
| Overview | [`index.md`](index.md) | How the organization approaches standardization: the hierarchy, its vocabulary, and the route to every page |
| Organizational principles | [`principles/`](principles/index.md) | Conventions universal across the organization |
| Architectures | [`architectures/`](architectures/index.md) | Technology-agnostic definitions of a domain's elements and rules |
| Standards | [`standards/<key>/`](standards/index.md) | Each standard: a technology-specific implementation of an architecture, with its own principles |
| Repositories | `standards/<key>/<repo>/` | Each member repository's documented details |

Three terms bind the levels. An **architecture** defines a domain's compositional elements and
the rules that bind them, independent of any technology. A **standard** implements an
architecture on a specific technology and declares the principles its member repositories share.
A **principle** is a singular convention attached to any level of the hierarchy. The narrowing
rule joins them: a lower level may enhance — tighten — a principle it derives from, and never
loosen it.

## Page metadata

Every page opens with YAML front matter. Common fields:

```yaml
key:  go-minimal        # stable identifier; matches the page's path segment
name: Go Minimal        # prose name
type: standard          # index | principle | architecture | standard | repository | page
```

Per-type fields:

- `type: principle` — `level`: `organization`, or the key of the standard or architecture the
  principle attaches to.
- `type: architecture` — `status`: `draft` | `active` | `superseded`.
- `type: standard` — `architecture`: the key of the architecture the standard implements;
  `status` as above; `derives`: the key of the standard it re-expresses, when it does.
- `type: repository` — `tier`: `core-sdk` | `infrastructure` | `sdk` | `template` | `reference`;
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
