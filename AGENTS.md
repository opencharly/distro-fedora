# box/fedora — AGENTS.md signpost (not the rule-set)

This submodule is **self-contained**: it OWNS the full Fedora base/builder stack
locally (`fedora` → `fedora-nonfree` → `fedora-builder`), the Fedora **GPU base
family** (`nvidia` / `python-ml`), the consumer showcase images (`fedora-coder` /
`charly-fedora` / `fedora-test`), the `sway-browser-vnc` desktop, and the
relocated Fedora-rooted app/fixture boxes (jupyter, jupyter-ml, comfyui, ollama,
unsloth-studio, immich, immich-ml, openwebui, hermes, web, check-pod, redis,
tier1/tier23, …) — all discovered under `box/`. It vendors **no shared candy
layers**: every candy is an `@github.com/opencharly/<layer-*|pod-*|plugin-*>[:subdir]:<tag>`
ref from its standalone candy repo, and TWO local candies are discovered via the
`discover:` block — the `candy/charly-marketplace` closure manifest, and the
`candy/fedora-skills` skill carrier that owns this repo's ten `skill:` entities
(their bodies live at `candy/fedora-skills/.agents/skills/<name>/SKILL.md`). Every box's base is a bare
LOCAL name (`base: fedora` / `fedora-nonfree` / `nvidia`); the distro/builder/init
build vocabulary is embedded in the `charly` binary. There is NO namespace import
(`import: []`). Main imports THIS repo under the `fedora` namespace to build the
relocated boxes; this repo imports nothing back (the former two-way main↔fedora
import is dissolved).

Each skill's prose is authored here, in the skill's own file; the `charly.yml`
`skill:` node keeps only the routing (`name`/`family`/`owner`/`triggers`). The
harness discovers the file natively and resolves it from the address form below,
so the addresses carry no path — do not add one here.

## Load these skills first (R0)

- `/charly-distros:fedora` — the Fedora 43 base image; root of the RPM hierarchy.
- `/charly-distros:fedora-builder`, `/charly-distros:fedora-nonfree`,
  `/charly-distros:rpmfusion` — builder + nonfree repos.
- `/charly-distros:charly-fedora`, `/charly-distros:fedora-test` — the showcase images.
- `/charly-coder:fedora-coder` — the dev image.
- `/charly-distros:nvidia`, `/charly-distros:cuda` — the Fedora GPU base (`nvidia`
  box) + CUDA toolkit; `python-ml` builds on it.
- `/charly-jupyter:jupyter-ml-notebook` — the CUDA ML JupyterLab box with the
  fine-tuning / Ollama / LLM-course notebooks.
- `/charly-selkies:sway-browser-vnc` — the minimal Sway + wayvnc + Chrome desktop.
- `/charly-distros:tutorial-shell`, `/charly-distros:valkey-test` — the teaching
  shell quoted end to end by opencharly.ai, and the Valkey test box.

**Authoritative rules live in the `opencharly` superproject's root `AGENTS.md`**
(R0–R10, hard-cutover, AI attribution, git-workflow). This file only signposts
and restates no rule. The multi-agent workflow is in `/charly-internals:agents`.
History lives in this repo's `CHANGELOG/` (one file per CalVer).
