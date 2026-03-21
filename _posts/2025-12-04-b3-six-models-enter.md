---
layout: post
title: "Six Models Enter, One Problem Wins: A Head-to-Head Segmentation Showdown"
description: "I trained YOLOv8, YOLOv11, Mask R-CNN, and two SAM hybrid pipelines on oil palm drone data. Here's what the numbers actually showed — including the result that surprised me most."
tags: [yolo, mask-rcnn, instance-segmentation, deep-learning, model-comparison, research]
date: 2025-12-04 08:00:00 +0700
---

When I started this research, the question seemed straightforward: which model is best for detecting oil palm trees from drone imagery? After training six models across eight resolution levels and generating hundreds of evaluation metrics, the answer turned out to be: *it depends — but in a very specific, operationally useful way.*

---

## Context / Problem

Instance segmentation for agricultural monitoring requires balancing three competing demands: detection accuracy (finding every tree), segmentation quality (drawing accurate masks around each crown), and computational efficiency (running fast enough to be deployable). No single model maximises all three simultaneously. The goal was to understand exactly where each model sits in this trade-off space — and what that means for practical deployment.

---

## The Six Contenders

### YOLOv8l-seg
Released in 2023, YOLOv8 introduced anchor-free detection with a decoupled head separating classification from localisation. The segmentation variant adds a lightweight ProtoNet branch that generates mask coefficients. It has 45.9M parameters and 105.0 GFLOPs — the heaviest of the YOLO family tested here.

### YOLOv11l-seg
YOLOv11 advances YOLOv8 with two key architectural additions: the **C3k2 block** (multi-scale convolution with improved gradient flow) and **C2PSA** (convolutional module with parallel spatial attention). At 27.6M parameters and 65.9 GFLOPs, it is 40% lighter than YOLOv8l while outperforming it on this dataset.

### YOLOv11-Ablation
A diagnostic variant of YOLOv11 with identical architecture, trained with a modified augmentation strategy to isolate the effect of the C2PSA attention mechanism on multi-GSD robustness. It performed unexpectedly well — more below.

### Mask R-CNN (ResNet-50)
The two-stage baseline. Mask R-CNN first generates region proposals, then classifies and segments each proposal separately. It has 43.9M parameters and 133.9 GFLOPs. It is the most computationally expensive model tested, but also the most studied architecture for instance segmentation in agriculture.

### Hybrid-v8-SAM and Hybrid-v11-SAM
These pipelines use a YOLO detector to generate bounding box prompts, which are then passed to **SAM 2.1** (Segment Anything Model, Meta AI) for high-quality mask generation. SAM 2.1 uses a hierarchical Hiera encoder with a memory mechanism. The combined pipeline has 251.6M parameters and 2,565.9 GFLOPs. (See B4 for the full paradox story.)

---

## Training Setup

| Parameter | Value |
|-----------|-------|
| Hardware | NVIDIA A100-SXM4, Google Colab Pro+ |
| Framework | PyTorch 2.9.0 + Ultralytics 8.3.251 |
| Optimizer | AdamW, lr=0.002, momentum=0.937 |
| Max epochs | 200 (early stopping: patience=30 on mAP@50-95) |
| Batch size | 16 |
| Image size | 640×640 px |
| Initialisation | COCO-pretrained weights |

YOLOv11l-seg ran the full 200 epochs (1.06 hours) and achieved MaskAP@50 of **0.880**. YOLOv8l-seg early-stopped at epoch 62 (0.68 hours) with MaskAP@50 of 0.859 — converged faster but to a lower ceiling. YOLOv11 weights are 55.9 MB vs 92.3 MB for YOLOv8 — 40% lighter.

---

## Results: Overall Mean (All 8 GSDs)

| Model | Mean F1 | Mean IoU | F1 Std Dev |
|-------|---------|----------|------------|
| Mask R-CNN | **0.727** | 0.751 | **0.093** |
| Hybrid-v11-SAM | 0.680 | 0.696 | 0.119 |
| YOLOv11-ablation | 0.667 | — | 0.134 |
| YOLOv11l-seg | 0.646 | **0.769** | 0.151 |
| Hybrid-v8-SAM | 0.635 | 0.692 | 0.128 |
| YOLOv8l-seg | 0.616 | 0.751 | 0.142 |

**Mask R-CNN leads on mean F1 and stability** (lowest std=0.093). It is the most consistent model across the full GSD range.

**YOLOv11l-seg leads on mean IoU** (0.769). When a tree is detected, YOLOv11 draws more accurate masks than any other model.

**The weight-accuracy trade-off is decisive.** YOLOv11l-seg is 40% lighter than YOLOv8l-seg and consistently outperforms it on both F1 and IoU. For new deployments, there is no reason to use v8 over v11.

---

## Results: Fine Resolution (0.03–0.04m GSD)

| Model | F1 @ 0.04m | IoU @ 0.04m |
|-------|------------|-------------|
| Mask R-CNN | 0.847 | 0.871 |
| **YOLOv11-ablation** | **0.832** | 0.882 |
| YOLOv11l-seg | 0.815 | **0.895** |
| YOLOv8l-seg | 0.804 | 0.867 |

At 0.04m specifically, **YOLOv11-ablation achieves the highest F1 (0.832)**. The ablation variant's modified augmentation schedule appears to particularly benefit performance at this specific resolution. For rapid census work at the recommended operating point, it is the strongest single-model choice.

---

## Efficiency Profile

| Model | Latency (ms) | FPS | Memory (MB) |
|-------|-------------|-----|-------------|
| YOLOv8l-seg | 12.4 | 80.8 | 2,229 |
| YOLOv11l-seg | 18.3 | 54.6 | 2,341 |
| YOLOv11-ablation | 18.9 | 52.8 | 2,445 |
| Mask R-CNN | 23.5 | 42.5 | 2,798 |
| Hybrid-v11-SAM | 418.3 | **2.4** | 4,341 |

YOLOv11 at 18.3ms is well within real-time range on GPU hardware. The Hybrid SAM pipeline at 418ms and 2.4 FPS is unsuitable for any real-time application.

---

## Notes

**Why does Mask R-CNN perform so consistently?** Two-stage detectors are inherently more stable across resolution variation because the region proposal network is less sensitive to absolute pixel scale than single-stage anchor-free detectors.

**How were models compared fairly?** SAM was used in zero-shot mode — not fine-tuned on oil palm data. This represents the realistic deployment scenario. Fine-tuning would likely improve results but removes the "foundation model out-of-the-box" argument.

All training configs, result CSVs, and PR curve plots are in the companion repository: [github.com/Sai21112000/oil-palm-instance-segmentation](https://github.com/Sai21112000/oil-palm-instance-segmentation).

---

*Part 3 of 6 in the Oil Palm AI series. Next: [B4 — The Hybrid Paradox](#)*
