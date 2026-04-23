---
layout: post
title: "Parameter Golf: From Zero to a Working Cloud Baseline"
description: "Reproducing OpenAI's Parameter Golf language model training challenge on RunPod — covering environment setup, SSH debugging, dataset wiring, and landing a val_bpb baseline on a single H100."
tags: [nlp, language-models, parameter-golf, openai, reproducibility, cloud-gpu]
date: 2026-04-24 06:00:00 +0700
---

I spent the past few days getting OpenAI's **Parameter Golf** challenge running end-to-end on a rented GPU pod. The goal was simple: reproduce the training pipeline, record a valid `val_bpb` metric, and establish a baseline I could iterate on.

What looked like "clone and run" turned into a classic environment debugging exercise — SSH key mismatches, missing repo trees, and wall-clock caps that stop training before you expect.

---

## Context

**Parameter Golf** is OpenAI's Model Craft challenge: train the best language model you can under a hard **artifact budget** (about **16 MB** of counted bytes — code plus compressed weights) and, for official leaderboard runs, a **training time** budget on high-end NVIDIA hardware. Models are scored on **validation bits per byte (`val_bpb`)** on a fixed FineWeb validation setup, using a published cached dataset and tokenizer so results are comparable.

The repo ships two entry points: **`train_gpt.py`** for **CUDA / PyTorch** (what you use on a GPU cloud box) and **`train_gpt_mlx.py`** for **Apple Silicon** with MLX for quick local iteration. The same challenge rules and data scripts apply; only the runtime stack changes.

---

## Problem

The work splits naturally into two environments: **your Mac** (good for repo layout, git, and MLX if Metal is available) and **a rented GPU pod** (where `nvidia-smi` and `torchrun` actually live). Early on, several things look like "one broken command" but are really **environment mismatches**: running GPU checks on macOS, expecting a password for SSH when the host only trusts keys, or training from a directory that looks like the repo but is not a full git clone.

Separately, **`val_bpb` is not one universal number**: it depends on how long you train, how much training data you stream, and whether you hit the script's wall-clock cap. A short or shard-limited run is a valid pipeline check; it is not automatically comparable to the published naive baseline on the leaderboard without matching configuration.

---

## Cause: The Real Blockers

**On the Mac**, `nvidia-smi` is missing because consumer macOS does not ship NVIDIA drivers — that is expected, not a broken install.

**For MLX**, training needs a Metal-capable session. Sandboxed or headless automation often reports no Metal device, so MLX fails at import or first GPU use even when the same laptop works fine in a normal desktop session.

**For RunPod**, the template advertises SSH on port 22, but exposed direct TCP ports often mean: the instance expects SSH public key auth, and the key your pod was provisioned with must match what you registered in RunPod. A direct `root@IP -p customport` attempt can fall through to password auth and fail with `Permission denied (publickey,password)` even when your local key file exists — because the server never authorized that key for that path. Using RunPod's `user@ssh.runpod.io` style endpoint matched the pod's key setup and dropped into a real shell.

Inside the pod, `/workspace/parameter-golf` was sometimes an empty or partial tree (not a git repo, no `train_gpt.py`, no `data/cached_challenge_fineweb.py`). That usually means the folder was created by the template or a prior step but the upstream repo was never cloned there (or was wiped). The fix is always the same: `git clone` the official repository into a clean path, then run paths relative to that clone.

---

## How It Works

**Bits per byte** is a compression-flavored view of average validation loss: lower is better, and it is comparable across tokenizer choices in this challenge's evaluation setup because the metric is defined in a tokenizer-aware way for scoring.

**`torchrun --standalone --nproc_per_node=1 train_gpt.py`** launches one process on one GPU, which is the right shape for a single H100 smoke or baseline run. Environment variables point the script at the cached dataset directory and SentencePiece model for the `sp1024` vocabulary variant.

The training script enforces a default wall-clock budget (on the order of ten minutes of training time). When that fires, logs show something like `stopping_early: wallclock_cap` and training stops before the configured maximum step count. That is intentional guardrail behavior, not a crash.

Dataset size is controlled by how many training shards you download. Fewer shards mean faster iteration and less disk; the full default pulls more tokens and behaves closer to the documented baseline story.

---

## Solution: Setup and Results

### On the Mac

Clone `parameter-golf`, create a Python venv, install the MLX-oriented stack from the README (`mlx`, `numpy`, `sentencepiece`, `huggingface-hub`, `datasets`, `tqdm`), and pull a small shard count first for smoke tests. If MLX cannot see Metal in your current terminal environment, treat the Mac as orchestration-only and move training to the pod.

### On RunPod

Use the Parameter Golf template image (Python 3.12, PyTorch with CUDA). SSH in via the method RunPod shows for your pod — for me, `ssh.runpod.io` with the pod-specific user worked where raw `root@IP:port` did not. Inside the machine:

1. `cd /workspace` and `git clone` the official repo if the tree is missing files.
2. Run `python3 data/cached_challenge_fineweb.py --variant sp1024` (optionally `--train-shards N` for smaller pulls).
3. Export `RUN_ID`, `DATA_PATH`, `TOKENIZER_PATH`, `VOCAB_SIZE`, then run `torchrun` as above. Put `RUN_ID` on its own line before piping to `tee` so the log filename expands correctly.
4. Grep the log for `final_int8_zlib_roundtrip_exact` (and related lines) to record `val_bpb`.

### Result

I landed in the **mid-1.34 `val_bpb` range** on capped runs — consistent with "real training loop, capped budget," and useful as a reproducibility anchor for my own fork. The published naive baseline in upstream `records/` remains the public reference point near **~1.22 `val_bpb`** on its stated setup; my number is not a drop-in replacement for that leaderboard row without matching every constraint.

### For GitHub and career collateral

Fork the upstream repo under your account, push a branch that holds notes and excerpts (not huge datasets), and keep language honest: **reproduced training and logged metrics**, not SOTA unless you later meet the challenge's submission bar.

---

## Notes

- **Direct TCP vs `ssh.runpod.io`:** both can be valid; which one accepts your key depends on how the pod was wired. If you see password prompts immediately, treat it as key provisioning, not "find a password in the UI."
- **Agent vs human terminal:** automated environments may lack GPU backends even when your laptop has them; don't debug MLX purely inside a sandbox if the symptom is "no Metal device."
- **Public claims:** tie any resume or blog sentence to a log line and commit hash when possible; wall-clock cap and shard count belong in the same breath as `val_bpb`.

---

*The run logs, notes, and setup instructions are at [github.com/Sai21112000/parameter-golf/runs](https://github.com/Sai21112000/parameter-golf/tree/runs/runpod-baseline-repro/runs).*
