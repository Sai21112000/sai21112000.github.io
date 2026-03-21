---
layout: post
title: "Building the Dataset Nobody Had: Annotation, Corruption, and the Split That Kept Everything Honest"
description: "How I built a multi-resolution oil palm segmentation dataset from scratch — including the 301 corrupted label files I had to fix before anything could run."
tags: [dataset, annotation, computer-vision, deep-learning, precision-agriculture, research]
date: 2025-11-28 08:00:00 +0700
---

Before any model gets trained, someone has to build the dataset. In most research papers, this gets a single paragraph. In practice, it consumed the majority of my project time — and included a crisis I didn't see coming.

This post documents the full pipeline: from raw UAV orthomosaic to clean, multi-resolution, spatially validated training data. It also covers the moment I discovered 71% of my label files were corrupted, and how I fixed them automatically before losing a week of work.

---

## Context / Problem

There is no publicly available instance segmentation dataset for oil palm at multiple ground sampling distances (GSD). Existing datasets either use bounding boxes instead of polygon masks, cover a single resolution, or are locked behind institutional access. If I wanted to study how detection performance varies with flight altitude, I had to build the data myself.

The requirements were specific:
- **Polygon-level annotations** (not bounding boxes) for instance segmentation
- **Multiple GSD levels** (0.03m through 0.20m) from the same plantation
- **Spatial independence** between training and test splits — no geographic overlap

---

## Source Data

The raw imagery came from a research project by Thailand's Agricultural Research Development Agency (ARDA) covering commercial oil palm plantations in Krabi Province, Southern Thailand. The sensor was a DJI Phantom 4 Pro — a 1-inch 20MP CMOS with a mechanical shutter — producing a native orthomosaic at approximately 0.054m GSD.

The plantation covered mature palms between 8 and 25 years old, with high canopy closure and significant frond overlap between adjacent trees. This is the hard version of the problem: if the model works here, it works in realistic operational conditions.

---

## How It Works: The Pipeline

### Step 1 — Tiling

A full orthomosaic is too large to feed directly into a neural network. I extracted fixed 640×640 pixel tiles with a 20% overlap between adjacent tiles to avoid cutting trees at patch edges. This produced 421 tiles across the plantation area.

### Step 2 — Manual Annotation (CVAT)

Each tile was annotated manually using CVAT (Computer Vision Annotation Tool), drawing polygon masks around each individual oil palm canopy — not the full canopy cluster, but each tree as a separate instance. The annotation protocol was consistent throughout:
- Include the full crown projection to the frond tips
- Exclude shadow and soil visible through frond gaps
- Label partially occluded trees only if more than 50% of the crown is visible

### Step 3 — Format Conversion: COCO JSON → YOLO TXT

CVAT exports in COCO JSON format. YOLO requires normalised polygon coordinates in plain text files, one file per image, with class ID 0 for oil palm. I wrote a conversion script to handle this transformation, including coordinate normalisation and polygon simplification.

### Step 4 — Multi-GSD Simulation

Rather than flying at each altitude, I simulated all eight GSD levels mathematically from the native orthomosaic using bilinear interpolation. For each target GSD, a scale ratio is computed and each tile is resampled accordingly. This generated 8× the original dataset volume from a single flight mission — 3,368 tile-label pairs in total.

---

## The Corruption Crisis

After converting all 421 label files to YOLO format, I ran a validation check before training. The result: **301 out of 421 label files were invalid**.

The cause was a subtle bug in how CVAT exported certain polygon types. When a polygon annotation had an odd number of coordinate pairs — which happens when CVAT inserts a closing point that doesn't match the opening vertex — the YOLO format parser rejected the file silently. The training script would simply skip the corrupted file without raising an error.

This is the most dangerous kind of bug: it doesn't crash anything, it just quietly removes 71% of your training data.

The fix was a repair script that:
1. Loaded each label file and parsed all polygon coordinate sequences
2. Detected any sequence with an odd vertex count
3. Removed the trailing duplicate coordinate to restore even parity
4. Rewrote the cleaned file in-place

All 301 files were repaired in under 10 seconds. The full repair script is in the companion repository.

---

## The Starfield Split: Zero Spatial Leakage

Standard train/test splits are done randomly — you shuffle the dataset and allocate 80% to training and 20% to testing. For spatial data, this is a serious methodological problem. Adjacent tiles from the same plantation area share soil type, lighting conditions, palm age, and planting density. If tile A goes to training and tile B (its immediate neighbour) goes to testing, the model has effectively "seen" the test area during training.

I used what I call the **Starfield Split**: tiles were assigned to train or test based on their spatial location ID — a unique identifier tied to a discrete geographic zone within the plantation. No two tiles from the same zone appear in both splits. This guarantees **zero spatial leakage** between training and test data.

The result is a more honest evaluation. When the model scores well on this split, it has genuinely generalised to unseen spatial regions — not just interpolated between neighbouring tiles it already memorised.

---

## Dataset Summary

| Property | Value |
|----------|-------|
| Native GSD | 0.054 m/pixel |
| Tile size | 640 × 640 px |
| Base tiles annotated | 421 |
| Labels corrupted (repaired) | 301 / 421 |
| GSD levels simulated | 8 (0.03–0.20m) |
| Total tile-label pairs | 3,368 |
| Annotation format | COCO JSON → YOLO TXT |
| Split method | Starfield (spatial zone holdout) |

---

## Notes

**Why not use semi-automatic annotation?** Tools like SAM can accelerate annotation through interactive segmentation. I chose fully manual annotation for the gold-standard training set to ensure mask quality — particularly for partially occluded crowns and edge trees where automated tools produce inconsistent boundaries. The thesis itself evaluates SAM's output quality, so using it for training labels would have introduced circular bias.

The full annotation pipeline — tiling script, format converter, corruption repair script, and Starfield Split implementation — is open source at [github.com/Sai21112000/oil-palm-dataset-pipeline](https://github.com/Sai21112000/oil-palm-dataset-pipeline).

---

*Part 2 of 6 in the Oil Palm AI series. Next: [B3 — Six Models Enter, One Problem Wins](#)*
