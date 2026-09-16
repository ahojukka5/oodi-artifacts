# oodi-artifacts

Privately-hosted Julia binary artifacts for early Oodi-ecosystem dependencies whose
binaries are not yet available from the public artifact CDN (JuliaBinaryWrappers /
Yggdrasil). Each artifact is attached as a release asset; the consuming package's
`Artifacts.toml` points its `[[download]]` at the asset URL.

## Hosted artifacts

Current published artifacts include:

- **libopencascade_cxxwrap — Linux x86_64/glibc**
  - release: `libopencascade_cxxwrap-707416b9`
  - Julia artifact tree: `707416b9331d65576938b0ef9cd93301079e8491`
  - rebuilt on EL8 so the wrapper remains compatible with the intended glibc baseline;
- **libopencascade_cxxwrap — macOS aarch64**
  - release: `libopencascade_cxxwrap-fe916c69`
  - Julia artifact tree: `fe916c696b41979301c71c01e398d3f6efb07466`;
- **libnetgen_cxxwrap**
  - latest published release in this repository: `libnetgen_cxxwrap-d2a7f166`;
  - older retained releases include `cf0507bc`, `890f20b2`, and `654a8ffa` for historical consumer revisions;
- **NGSolveNetgen**
  - retained release: `NGSolveNetgen-22b6752d`.

The old `libopencascade_cxxwrap-19b04788` release is retained as historical
provenance only. It is not the current Monge wrapper and must not be presented as
the installation target.

Release assets are immutable inputs to consuming `Artifacts.toml` files. A consumer
should pin the exact artifact tree and download checksum it has qualified rather than
infer compatibility from the newest release name alone.

## Publishing OpenCascadeCxxWrap

`.github/workflows/publish-opencascade-cxxwrap.yml` owns the current temporary
**Linux x86_64/glibc OpenCascade wrapper** publication path until the wrapper moves to
Yggdrasil. Other artifacts listed above are hosted here but are not currently rebuilt
by this workflow.

The workflow pins both the wrapper source revision and the Monge revision used for
qualification.

The workflow is **manual only** (`workflow_dispatch`). A native OpenCASCADE
build occupies a self-hosted runner for a long time and republishes an
immutable, rarely-regenerated binary, so dispatch it when the pinned wrapper
revision actually changes -- not on every edit to the workflow file.

It builds the Linux x86_64/glibc artifact once and proves the strict raw
`GeomFill_Gordon` fixture together with the public `gordon_surface` rectangle
fixture. The deterministic archive, provenance record, and build log are
uploaded as workflow artifacts.

Reproducibility is not re-checked by a second native build in the same run. The
immutable-tag rule below catches a rerun whose bytes differ from an already-published
tag; first-publication reproducibility therefore rests on the pinned source/toolchain
inputs plus the qualification evidence recorded by the workflow.

The qualification publishes a release named from the Julia artifact tree hash.
Existing tags are immutable by policy: a rerun succeeds only when both the archive
bytes and provenance are identical. Every release records the wrapper source commit,
qualification commit, platform, archive SHA-256, and extracted Julia artifact tree
hash.

Monge's `Artifacts.toml` should be updated only after that release succeeds and
the published asset can be installed from a clean depot.
