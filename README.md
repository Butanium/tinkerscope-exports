# tinkerscope-exports

Portable [tinkerscope](https://github.com/Butanium/tinkerscope) **share packs** —
a single YAML bundling public checkpoints + default sampling params + saved
workspaces (branch trees), so you can reproduce a setup with no local training
runs.

## Packs

| File | What's in it |
|---|---|
| [`cot-prefilling.yaml`](./cot-prefilling.yaml) | CoT-prefilling probes on 4 base models (Inkling · Nemotron-3-Ultra · DeepSeek-V3.1 · Kimi-K2.6). 4-panel workspace, 62 nodes / 13 branch points, with raw request/response metadata (the "Raw" view). No token-logprob blobs. |

## Run one on a fresh machine (one-liner)

Installs tinkerscope from source with [`uv`](https://docs.astral.sh/uv/), makes a
workdir, and serves seeded from the pack (the workdir is the scan root, so state
persists there):

```bash
mkdir cot-prefilling && cd cot-prefilling && uvx --from git+https://github.com/Butanium/tinkerscope tinkerscope --pack https://raw.githubusercontent.com/Butanium/tinkerscope-exports/main/cot-prefilling.yaml
```

It prints a local URL (e.g. `http://127.0.0.1:8765`) — open it and pick the
**CoT prefilling** workspace.

### Notes

- **Viewing** the exported workspace (messages, branch trees, the Raw view)
  needs nothing — no API key, no GPU.
- **Sampling** the models yourself needs a `TINKER_API_KEY` in the environment.
  The Inkling base model additionally needs tinkerscope installed with the
  cookbook `[inkling]` extra.
- `--pack` also takes a local path instead of a URL.
