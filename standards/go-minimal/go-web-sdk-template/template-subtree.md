---
key: template-subtree
name: The template subtree
type: page
repo: go-web-sdk-template
---

# The template subtree

Why the template's module is rooted at `template/` rather than the repository root, and the
duties the layout creates.

## The generation boundary is structural

A module's zip is its `go.mod`-rooted subtree: the proxy packs every committed file below
`go.mod` and nothing above it, and generation copies that zip with the module path rewritten.
With the module at the repository root, every generated service would inherit the template's
management layer: its working context, agent configuration, and changelog. Excluding any of it
would require post-generation cleanup instructions. With the module at `template/`, the
boundary is the directory line: a generated service receives the subtree, the management layer
cannot ship, and no cleanup instructions exist to be skipped.

## Costs accepted

- The module path gains a `/template` segment. Generated services never see it: generation
  rewrites the whole path.
- Version tags are prefixed `template/v*`. The prefix is fixed by the proxy, which resolves a
  subdirectory module's versions from tags prefixed with exactly that subdirectory. The tag is
  also the one place the directory name is externally visible, which is why the directory is
  named `template` rather than `src`.
- The root and subtree each keep their own ignore rules and CI workflow. The root workflow runs
  inside `template/`; the subtree copies serve the generated service and are inert in the
  template repository. A change to either copy is applied to the other in the same commit.

## What the layers contain

The root: the working context, the template's README and changelog (the source of the release
notes), the committed `go.work` that resolves the module from the root, the CI and release
workflows, and the license. The subtree: the service source, its configuration files, the
developer tasks, the starter README addressed to the generated service, and the license; the
Go documentation site requires a license inside the module subtree to render documentation.

Deliberately absent from the subtree: release automation and a changelog. Both encode
conventions the generated service's author owns.
