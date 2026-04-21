---
layout: post
title: "Reproducing the LPCVC 2026 Track 1 Baseline on Qualcomm AI Hub"
description: "A walkthrough of reproducing a working CLIP image-to-text retrieval baseline on XR2 Gen 2, covering environment fixes, SSL certificates, dataset paths, and preprocessing gotchas."
tags: [computer-vision, edge-ai, qualcomm, clip, lpcvc, reproducibility]
date: 2026-04-22 08:00:00 +0700
---

I spent the last few days reproducing the LPCVC 2026 Track 1 sample solution on Qualcomm AI Hub. The goal was straightforward: get the full CLIP-based image-to-text retrieval pipeline running end-to-end and record a valid Recall@10 baseline on the XR2 Gen 2 (Proxy) target.

What I thought would be a quick exercise turned into a classic reproducibility journey — the blockers were not model architecture at all. They were environment issues, SSL certificates, path mismatches, and subtle preprocessing assumptions.

---

## Context / Problem

The LPCVC 2026 Track 1 sample solution provides a working baseline for image-to-text retrieval using CLIP. The task: given an image of a natural scene (from the NaturalScenes dataset), retrieve the most relevant textual descriptions from a pool of candidate captions.

The evaluation metric is **Recall@10** — for each image, we check if its ground-truth caption appears in the top-10 most similar texts ranked by cosine similarity between image and text embeddings.

Target platform: **XR2 Gen 2 (Proxy)** via Qualcomm AI Hub. Workflow: export → compile/profile → upload dataset → inference → compute metrics.

---

## Cause: The Real Blockers

Despite having working code, I hit three environment-level issues before getting valid results:

1. **Python package mismatch** — `clip` install failed due to missing `pkg_resources` and build tooling incompatibility.

2. **SSL certificate trust failure** — downloading CLIP weights from PyTorch Hub failed with `ssl.SSLCertVerificationError`. This blocked ONNX export entirely.

3. **Dataset path mismatch** — the scripts expected `dataset/...` paths relative to the sample directory, but my actual data lived in a parent `Track 1/` folder.

Later, I discovered a **quality concern**: the sample preprocessing used only `/255.0` scaling, not full CLIP mean/std normalization. This was enough to skew reproducibility even before model evaluation.

---

## How It Works: The Pipeline

The baseline workflow has seven practical steps:

1. **Configure AI Hub** and verify target device visibility  
2. **Set up venv** and install requirements  
3. **Export CLIP encoders** to ONNX (image + text)  
4. **Compile and profile** both encoders on XR2 Gen 2 (Proxy)  
5. **Upload image and text datasets** to AI Hub  
6. **Run inference jobs** for both encoders  
7. **Compute Recall@10** from downloaded embeddings against ground-truth CSVs

At a high level:
- Image encoder produces a **512-D embedding** per image
- Text encoder produces a **512-D embedding** per text prompt  
- After L2 normalization, **cosine similarity** ranks text candidates for each image
- Recall@10 measures how often ground-truth text IDs appear in the top 10

---

## Solution: Fixes and Results

### 1) Environment fixes

Created a Python 3.11 venv in the `sample-solution` directory. The `clip` package failed to install due to build isolation issues. Fixed by:

```bash
pip install setuptools wheel
pip install git+https://github.com/openai/CLIP.git --no-build-isolation
```

Then installed remaining requirements successfully.

### 2) SSL certificate failure (critical)

`export_onnx.py` initially failed with:

```
ssl.SSLCertVerificationError
urllib.error.URLError: certificate verify failed: self signed certificate in certificate chain
```

Root cause: Python.org macOS cert trust path mismatch in this local setup.

Fix: Set `SSL_CERT_FILE` to the certifi CA bundle in the venv activation flow. After that, CLIP weights downloaded and ONNX export worked.

### 3) Dataset path fix

The scripts expected `dataset/...` inside the sample directory. My data was under `Track 1/`.

Fix: Created a symlink:

```bash
ln -s "../Track 1" dataset
```

Verified CSVs resolve correctly relative to this structure.

### 4) ONNX export success

```bash
python export_onnx.py
```

Produced:
- `exported_onnx/image_encoder.onnx` (+ external data)
- `exported_onnx/text_encoder.onnx` (+ external data)

### 5) Compile/profile on AI Hub

**Compile jobs:**
- Image compile job: `jpx7qd48g` → model `mq8g1zegm`
- Text compile job: `j5mw7dm7p` → model `mmr57e40n`

**Profile jobs:**
- Image profile: `j5q7nmleg`
- Text profile: `jgl0d1y2g`

Confirmed text input dtype compatibility via `--truncate_64bit_io` (int64 to int32 handling in the TFLite pipeline).

### 6) Dataset upload + initial inference

**Initial dataset uploads:**
- Image dataset: `d9ww165o9`
- Text dataset: `d7l0dr617`

**Initial inference jobs:**
- Text inference: `j57jm78q5`
- Image inference: `jpv189y7p`

**Initial result:**
```
Recall@10 = 0.7260416666666666
```

### 7) Preprocessing correction and re-test

The sample code used simple `/255.0` scaling. CLIP was trained with standard ImageNet-style normalization.

Updated preprocessing to:

```python
image = image / 255.0
image = (image - CLIP_MEAN) / CLIP_STD
```

**Re-uploaded datasets:**
- New image dataset: `d91yo3gx9`
- New text dataset: `d74jerxz7`

**Re-ran inference:**
- Text inference: `jg93rd0lg`
- Image inference: `j5mw78qwp`

**Updated result:**
```
Recall@10 = 0.7299107142857143
```

Change was small (+0.0039 absolute), so normalization was correct to apply, but not the main accuracy lever for this dataset.

### 8) Submission readiness

- Added `share_for_submission.py` to share compile jobs with evaluation email before LPCVC submission
- Updated `.gitignore` to avoid leaking large artifacts (ONNX files, datasets, etc.)
- Initialized repo and pushed to: [github.com/Sai21112000/LPCVC-2026-track1](https://github.com/Sai21112000/LPCVC-2026-track1)

---

## Notes

**Latency and memory on XR2 Gen 2 (Proxy):**
- Image encoder: **29.3 ms**, peak memory 0–628 MB
- Text encoder: **5.4 ms**, peak memory 0–185 MB

**Final reproduced baseline:**
```
Recall@10 = 0.7299
```

**Key insight:** Reproducibility depended more on environment correctness (certs, paths, IDs, data wiring) than on model code changes in early phases.

**Next steps:** The baseline is now solid. Optimization direction is likely prompt strategy, backbone variants, or quantization — not just preprocessing tweaks. The foundation is ready for actual model improvements now that the pipeline is reliable.

---

*The repo with all scripts and setup instructions is at [github.com/Sai21112000/LPCVC-2026-track1](https://github.com/Sai21112000/LPCVC-2026-track1).*
