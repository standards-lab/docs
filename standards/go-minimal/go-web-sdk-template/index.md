---
key: go-web-sdk-template
name: go-web-sdk-template
type: repository
tier: template
repo: https://github.com/standards-lab/go-web-sdk-template
standard: go-minimal
---

# go-web-sdk-template

The web service template of [Go Minimal](../index.md): scaffolds an initial Go Minimal web
service application, and the first code expression of the
[Elemental Architecture](../../../architectures/elemental-architecture/index.md)'s application
layout. A generated service starts as a minimal runnable web service on go-core and go-web-sdk
at pinned releases: layered configuration, the cold/hot start lifecycle, the liveness and
readiness probes, and the three build points a service grows from.

New services are generated with `gonew`, which copies the template module and rewrites its
path; the copy is a running service from the first build:

```sh
gonew github.com/standards-lab/go-web-sdk-template/template@latest example.com/newsvc
```

The template is engine-free: no data engine declared, no provider imported. A generated service
selects its providers in its own composition root. The template stays minimal and stable;
development concentrates in the SDKs, and a generated service keeps pace by updating its
go-core and go-web-sdk pins.

## Design

- [The baseline](baseline.md) — the architecture the template scaffolds: the composition root's
  three manifests and the machinery they feed.
- [The elements in the template](elements.md) — how the baseline realizes the Elemental
  Architecture's elements.
- [The template subtree](template-subtree.md) — why the module is rooted at `template/`, and the
  duties the layout creates.
