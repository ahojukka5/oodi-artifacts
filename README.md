# oodi-artifacts

Privately hosted Julia binary artifacts for early Oodi dependencies whose
binaries are not yet available from JuliaBinaryWrappers/Yggdrasil. Release
assets are immutable inputs to the consuming package's `Artifacts.toml`.

## Artifact policy

Every release must record:

- the exact source commits and Julia version used for the build;
- the extracted Julia artifact tree hash;
- the release archive SHA-256;
- a load or execution smoke test against the real consuming environment.

A release is not accepted merely because an archive was uploaded. The build
must be reproducible, and the consumer must validate both the archive checksum
and extracted tree hash.

## OpenCascade CxxWrap

`.github/workflows/build-opencascade-wrapper.yml` rebuilds
`libopencascade_cxxwrap` against Monge's pinned `OCCT_jll` environment. It uses
exact Monge and OpenCascadeCxxWrap commits, a clean Julia depot, and Monge's
explicit native build path.

Before publishing, the workflow:

1. rejects wrappers that still depend on unavailable `libTKDECascade`;
2. starts a new Julia process;
3. loads Monge and constructs a unit box;
4. tags the release by the resulting Julia artifact tree hash.

The workflow can be run manually after updating its pinned inputs. The initial
branch trigger exists only to bootstrap the repaired artifact for
`ahojukka5/Monge.jl#114`.

## Existing releases

- `libopencascade_cxxwrap-19b04788` is retained for provenance but is not
  compatible with Monge's current `OCCT_jll` 7.9.3 environment because it
  depends on `libTKDECascade.so.7.9`.
- `libnetgen_cxxwrap-890f20b2` hosts the early Netgen wrapper artifact.
