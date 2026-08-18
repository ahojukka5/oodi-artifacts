# oodi-artifacts

Privately-hosted Julia binary artifacts for early Oodi dependencies whose
binaries are not yet available from the public artifact CDN (JuliaBinaryWrappers /
Yggdrasil). Each artifact is attached as a release asset; the consuming package's
`Artifacts.toml` points its `[[download]]` at the asset URL.

- **libopencascade_cxxwrap** (`git-tree-sha1 19b047888fc9c9e148727f88ca0867016974fdf6`)
  — initial OpenCASCADE CxxWrap binary for Monge.jl, pending Yggdrasil merge.

## Publishing OpenCascadeCxxWrap

`.github/workflows/publish-opencascade-cxxwrap.yml` owns the temporary binary
publication path until the wrapper moves to Yggdrasil. The workflow pins both
the wrapper source revision and the Monge revision used for qualification.

The workflow is **manual only** (`workflow_dispatch`). A native OpenCASCADE
build occupies a self-hosted runner for a long time and republishes an
immutable, rarely-regenerated binary, so dispatch it when the pinned wrapper
revision actually changes -- not on every edit to the workflow file.

It builds the Linux x86_64/glibc artifact once and proves the strict raw
`GeomFill_Gordon` fixture together with the public `gordon_surface` rectangle
fixture. The deterministic archive, provenance record, and build log are
uploaded as workflow artifacts.

Reproducibility is not re-checked per run: the immutable-tag rule below already
catches a build whose bytes differ from what a tag holds.

The same qualification then publishes a release named from the Julia artifact
tree hash. Existing tags are immutable: a rerun
succeeds only when both the archive bytes and provenance are identical. Every
release records the wrapper source commit, qualification commit, platform,
archive SHA-256, and extracted Julia artifact tree hash.

Monge's `Artifacts.toml` should be updated only after that release succeeds and
the published asset can be installed from a clean depot.
