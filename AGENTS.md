# AGENTS.md — layer-nvenc-headers

Standalone candy repo for the `nvenc-headers` build-time layer — the NVIDIA
Video Codec SDK headers (`ffnvcodec` / `nvEncodeAPI.h`) an NVENC-capable encoder
compiles against, consumed by the `cuda-arch-builder` image. The candy lives in
`charly.yml` at the repo root: the `distro:` package arm and the `plan:`
`check:` assertions.

This repo has **no `skill:` entity** in `charly.yml`, so there is no dedicated
owning skill projected into the marketplace corpus. The gap is recorded against
`opencharly/opencharly#291` (the batch that authors missing `skill:` entities).

Canonical files:

- `charly.yml` — the `nvenc-headers:` candy entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-distros:cuda` — the closest owning procedure: the CUDA toolkit layer
  this header complements, and the NVENC build path. Load before editing or
  troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package/repo
  sections, and service declarations). Load before editing any entity field or
  plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence — they assert
  the header, the pkg-config descriptor, the entry point, and the package are
  present in the built image.

## Modify this repo

- There is no `skill:` entity to keep in sync; if one is added (per #291), it
  must be edited together with the candy entity in the same change.
- Keep the header out of the shared `cuda` layer: it is build-time only, and
  moving it would cascade-rebuild every CUDA runtime image.
- New behaviour claims belong in the `plan:` as an observable `check:` step.

## Landing

- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo — read it
  before landing.
- Release history lives in `CHANGELOG/`.
