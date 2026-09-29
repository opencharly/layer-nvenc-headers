# nvenc-headers

NVIDIA Video Codec SDK headers as a charly *build-time* layer — the `ffnvcodec`
/ `nvEncodeAPI.h` headers an NVENC-capable encoder compiles against.

The `nvenc-headers` candy installs the `ffnvcodec-headers` (nv-codec-headers)
package on Arch, which ships the NVENC/NVDEC API headers under
`/usr/include/ffnvcodec` plus the `ffnvcodec.pc` pkg-config descriptor. It is
used by the `cuda-arch-builder` image so pixelflux's `nvenc-sys` crate can
compile the real `NvencEncoder`.

It is kept **out of the shared `cuda` layer** deliberately: the many CUDA
runtime images (comfyui, jupyter-ml, immich-ml, …) do not need a build-only
header, and adding it to `cuda` would cascade-rebuild them all.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `nvenc-headers` |
| Package | `ffnvcodec-headers` (Arch) |
| Header | `/usr/include/ffnvcodec/nvEncodeAPI.h` |
| pkg-config | `/usr/lib/pkgconfig/ffnvcodec.pc` |
| Entry point | `NvEncodeAPICreateInstance` |
| Service / port | none |

## How to use it

Compose the layer in a **builder** image's `candy:` list. The canonical consumer
is `cuda-arch-builder`:

```yaml
cuda-arch-builder:
  candy:
    base: arch-builder
    candy:
      - '@github.com/opencharly/layer-nvenc-headers:v2026.239.1627'
      # ... cuda and the rest of the builder toolchain
```

The headers are present at build time only; they are not needed at runtime.

## Layout

- `charly.yml` — the `nvenc-headers:` candy entity: the `distro:` package arm
  and the `plan:` `check:` assertions.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

This repo carries no `skill:` entity of its own; `/charly-distros:cuda` is the closest
family owning procedure.

- `/charly-distros:cuda` — the CUDA toolkit layer this header complements.
- `/charly-internals:generate-source` — Containerfile generation for the builder.
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella.
