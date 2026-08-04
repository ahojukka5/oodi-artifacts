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

For pull requests, it builds the Linux x86_64/glibc artifact twice and requires
both builds to produce the same Julia artifact tree. It then proves the strict
raw `GeomFill_Gordon` fixture together with the public `gordon_surface`
rectangle fixture. The deterministic archive, provenance record, and build logs
are uploaded as workflow artifacts without publishing a release.

After the workflow reaches `main`, the same qualification publishes a release
named from the Julia artifact tree hash. Existing tags are immutable: a rerun
succeeds only when both the archive bytes and provenance are identical. Every
release records the wrapper source commit, qualification commit, platform,
archive SHA-256, and extracted Julia artifact tree hash.

Monge's `Artifacts.toml` should be updated only after that release succeeds and
the published asset can be installed from a clean depot.
