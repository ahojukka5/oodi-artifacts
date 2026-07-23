# oodi-artifacts

Privately-hosted Julia binary artifacts for early Oodi dependencies whose
binaries are not yet available from the public artifact CDN (JuliaBinaryWrappers /
Yggdrasil). Each artifact is attached as a release asset; the consuming package's
`Artifacts.toml` points its `[[download]]` at the asset URL.

- **libopencascade_cxxwrap** (`git-tree-sha1 19b047888fc9c9e148727f88ca0867016974fdf6`)
  — OpenCASCADE cxxwrap binary for Monge.jl, pending Yggdrasil merge.
