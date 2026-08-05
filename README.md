# tinkerscope-exports

Published snapshots from [tinkerscope](https://github.com/Butanium/tinkerscope) — a
**hosted read-only viewer** plus the **share packs** it opens. A pack is a single YAML
bundling public checkpoints + default sampling params + saved workspaces (branch trees),
so you can reproduce a setup with no local training runs.

## Open one in the browser — nothing to install

**→ [butanium.github.io/tinkerscope-exports](https://butanium.github.io/tinkerscope-exports/)**

| Pack | Direct link | What's in it |
|---|---|---|
| `value-guarding-v2.yaml.gz` | [open](https://butanium.github.io/tinkerscope-exports/viewer/?w=https://raw.githubusercontent.com/Butanium/tinkerscope-exports/main/value-guarding-v2.yaml.gz) | Value-guarding probes across the weird-personas checkpoints. 818 nodes, 766 with **per-token logprobs** — so the token inspector and the chart's *first token* mode work. 18 MB gzipped. |
| `cot-prefilling.yaml` | [open](https://butanium.github.io/tinkerscope-exports/viewer/?w=https://raw.githubusercontent.com/Butanium/tinkerscope-exports/main/cot-prefilling.yaml) | CoT-prefilling probes on 4 base models (Inkling · Nemotron-3-Ultra · DeepSeek-V3.1 · Kimi-K2.6). 4-panel workspace, 62 nodes / 13 branch points, with the raw request/response view. No logprobs. |

The viewer reads **any** tinkerscope pack, not only the ones here:

- point it at a URL — `…/viewer/?w=<your pack url>` (the host must allow cross-origin
  reads; `raw.githubusercontent.com` does);
- or open a pack from your own computer with the **⤒** button next to the workspace
  picker, or by dropping the file on the page. Nothing is uploaded — it is read and kept
  in your browser.

Installed packs live in that browser's IndexedDB and persist across reloads. Sampling is
off: a static site has no backend and no key.

## Run one locally instead

Installs [tinkerscope from PyPI](https://pypi.org/project/tinkerscope/) with
[`uv`](https://docs.astral.sh/uv/), makes a workdir, and serves seeded from the pack
(the workdir is the scan root, so state persists there):

```bash
mkdir cot-prefilling && cd cot-prefilling && uvx tinkerscope --pack https://raw.githubusercontent.com/Butanium/tinkerscope-exports/main/cot-prefilling.yaml
```

It prints a local URL (e.g. `http://127.0.0.1:8765`) — open it and pick the workspace.

### Notes

- **Viewing** a pack (messages, branch trees, the Raw view, token probabilities where
  the pack carries them) needs nothing — no API key, no GPU.
- **Sampling** the models yourself needs a `TINKER_API_KEY` in the environment. The
  cookbook's `[inkling]` extra — what makes the Inkling base model render correctly —
  is a hard dependency of tinkerscope, so any install already has it.
- `--pack` takes a local path or a URL, gzipped or not.

## What's in this repo

| Path | |
|---|---|
| `docs/` | what GitHub Pages serves — `docs/viewer/` is the static tinkerscope, `docs/index.html` the landing page |
| `*.yaml` / `*.yaml.gz` | the share packs themselves, linked by `?w=` above |

Regenerate with `tinkerscope site export docs/viewer --title tinkerscope` and
`tinkerscope pack export <name>.yaml.gz --workspace "<name>" --logprobs`.
