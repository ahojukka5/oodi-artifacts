# oodi-artifacts

Privately hosted Julia binary artifacts for early Oodi dependencies whose
binaries are not yet available from JuliaBinaryWrappers/Yggdrasil. Release
assets are immutable inputs to the consuming package's `Artifacts.toml`.

## Artifact policy

Every native release must record:

- the exact consumer and wrapper source commits;
- the Julia version and supported platform used for the build;
- the extracted Julia artifact tree hash;
- the deterministic release archive SHA-256;
- a load or execution test against the real consuming environment.

A release is not accepted merely because an archive was uploaded. The consumer
must validate both the archive checksum and extracted tree hash, and the wrapper
must be qualified against the public capabilities that depend on it.

## OpenCascade CxxWrap

`.github/workflows/publish-occt-wrapper.yml` rebuilds
`libopencascade_cxxwrap` against Monge's pinned `OCCT_jll` environment. It uses
exact Monge and OpenCascadeCxxWrap commits and an isolated Julia depot.

Before publishing, the workflow:

1. rejects wrappers that depend on unavailable `libTKDECascade`;
2. starts a new Julia process and loads Monge;
3. requires p-curve, helix, and Gordon bindings;
4. runs Monge's native and public qualification suite;
5. generates the hydraulic-manifold audit and render evidence;
6. creates a deterministic archive tagged by its artifact tree hash;
7. records hashes, source revisions, and the workflow run in a GitHub issue.

Pull requests execute the qualification path but cannot publish. Immutable
release creation is restricted to trusted pushes in this repository.

## Existing releases

- `libopencascade_cxxwrap-19b04788` is retained for provenance but is not a
  complete current Monge backend. It was built against a foreign OCCT runtime,
  depends on `libTKDECascade.so.7.9`, and predates current p-curve, helix, and
  Gordon bindings.
- `libnetgen_cxxwrap-890f20b2` hosts the early Netgen wrapper artifact.
