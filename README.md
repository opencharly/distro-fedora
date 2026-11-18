# opencharly/distro-fedora

The **Fedora image-family** for [OpenCharly](https://github.com/opencharly/charly),
split into its own repository and mounted as a git submodule at `box/fedora`
of the main repo.

## What's here

This submodule is **self-contained** — it owns its entire Fedora stack locally:

| Kind | Entries |
|---|---|
| Base / builder boxes | `fedora`, `fedora-nonfree`, `fedora-builder` |
| GPU base boxes | `nvidia`, `python-ml` |
| Showcase image | `charly-fedora` (minimal charly toolchain) |
| Desktop | `sway-browser-vnc` (minimal Sway + wayvnc + Chrome) |
| Relocated app / fixture boxes | `jupyter`, `jupyter-ml-notebook`, `comfyui`, `unsloth-studio`, `openwebui-qdrant`, `hermes`, `web`, `check-pod`, `redis`, `tier1`, `tier23`, … (many Fedora-rooted boxes, all discovered under `box/`) |

Every box's base is a bare **local** name (`base: fedora` / `fedora-nonfree` /
`nvidia`); the distro/builder/init build vocabulary is embedded in the `charly`
binary.

## Composition — local bases, candies by reference

This repo owns its bases and build targets locally, but **vendors no shared candy layers**:

- the Fedora base/builder stack (`fedora` → `fedora-nonfree` → `fedora-builder`)
  and the GPU base (`nvidia` / `python-ml`) are **local boxes** under `box/`,
  referenced by bare name (`base: fedora`, `base: nvidia`);
- every shared candy is an `@github.com/opencharly/<layer-*|pod-*|plugin-*>[:subdir]:<tag>`
  ref into its standalone candy repo — ONE local candy is discovered via the
  `discover:` block: `candy/fedora-skills` (the skill carrier that owns this repo's
  ten `skill:` entities, so each skill's prose is readable and reaches
  `ai.opencharly.skill` — opencharly/sdk#434). A local copy of
  opencharly/marketplace's refs list at `candy/charly-marketplace` was deleted: it
  drifted and could name a deleted tag, aborting the corpus generator
  (opencharly/marketplace#613), and the closure resolves transitively from each
  `@github` ref's own manifest;
- the distro/builder/init build vocabulary is embedded in the `charly` binary
  (no remote build-config import).

There is **no namespace import** (`import: []`); nothing here depends on the main
repo except the `@github` candy refs.

## Coupling with main (post-inversion, one-directional)

After the box-inversion cutover this repo is the canonical home of the Fedora
base stack and every Fedora-rooted box, so the dependency is **one-directional**:

- **main → fedora**: the main `opencharly` repo imports this repo under the
  `fedora` namespace to build the relocated boxes;
- **fedora → main**: only the `@github` candy refs into the shared candy library.

This repo imports nothing back. The image DAG stays acyclic
(`charly-fedora → fedora → quay.io/fedora/fedora:43`).

## Build

```bash
# Inside the submodule (the build verb defaults to charly.yml):
charly box build charly-fedora

# From the parent opencharly repo:
charly -C box/fedora box build charly-fedora

# Standalone, against the published repo:
charly --repo opencharly/distro-fedora box build charly-fedora
```

The first build resolves the upstream github references into
`~/.cache/charly/repos/` and materializes the referenced layers under
`.build/_layers/`.

## Requirements

A build of any image here fetches from the upstream repo, so it needs network
access and a `charly` recent enough to understand the config's schema version
(`charly` hard-fails with a "newer than this charly supports" message if the config
schema is newer than the binary supports).

---
*Assisted-by: Claude*
