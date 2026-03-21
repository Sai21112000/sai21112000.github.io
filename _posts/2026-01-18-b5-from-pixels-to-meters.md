---
layout: post
title: "From Pixels to Meters: The Math Behind Canopy Size Estimation"
description: "A segmentation mask is a collection of pixels. Here's how we convert that into a physical canopy area in square metres and an equivalent diameter — with the full derivation and error analysis."
tags: [canopy-estimation, applied-mathematics, computer-vision, precision-agriculture, shoelace-algorithm, research]
date: 2025-01-18 08:00:00 +0700
---

The goal of instance segmentation in precision agriculture isn't to produce a pretty coloured mask on a screen. It's to extract a measurement — a real, physical number that a plantation manager can use to make decisions. How big is this canopy? Is it growing or shrinking compared to last season? Is this tree underperforming relative to its neighbours?

Answering those questions requires a principled conversion from pixel-space to physical space.

---

## Context / Problem

A segmentation model outputs a binary mask: a set of pixels assigned to a given oil palm instance. Each pixel has no inherent physical scale. To get area in square metres or diameter in metres, you need to translate pixel counts into real-world dimensions using one parameter: the **Ground Sampling Distance (GSD)**.

Any error in the segmentation mask — missed frond tips, included shadow pixels, merged boundaries — propagates directly into the measurement. A 10% mask area error becomes a 10% area measurement error, and approximately a 5% diameter error (since diameter scales as the square root of area). Getting the mask right is not just a detection problem; it's a measurement calibration problem.

---

## How It Works: The Three Formulas

### Step 1 — Pixel Area to Physical Area

The physical area represented by a single pixel is the square of the GSD:

```
pixel_area = GSD²  (m² per pixel)
```

To compute the total physical area of a segmented canopy, count all pixels belonging to that instance mask and multiply:

```
A_canopy = N_pixels × GSD²
```

For example: at 0.04m GSD, a canopy mask containing 4,500 pixels represents:

```
A_canopy = 4500 × (0.04)² = 4500 × 0.0016 = 7.2 m²
```

### Step 2 — The Shoelace Algorithm

For polygon-based masks, the area can be computed directly from vertex coordinates using the **Shoelace Algorithm** (Gauss's area formula). Given a polygon with vertices (x₁,y₁) through (xₙ,yₙ):

```
A = 0.5 × |Σ(xᵢ × yᵢ₊₁ − xᵢ₊₁ × yᵢ)|
```

The sum wraps around (last vertex connects back to the first). This formula works for any simple polygon — convex or concave — and is exact. The result is in pixel² units, then multiplied by GSD² to convert to m².

### Step 3 — Equivalent Diameter

Oil palm canopies are irregular star-shaped polygons. The **equivalent diameter** is defined as the diameter of a perfect circle with the same area as the measured canopy:

```
D_equiv = 2 × √(A_canopy / π)
```

This standardises any irregular shape into a comparable linear metric and connects directly to existing forestry allometric equations.

---

## Validation: RMSE and MAE Results

### Diameter RMSE by GSD (Best Model per Level)

| GSD (m) | Best Model | Diameter RMSE (m) | Diameter MAE (m) |
|---------|------------|-------------------|------------------|
| 0.03    | Hybrid-v8-SAM | **1.42** | 1.32 |
| 0.04    | YOLOv8l-seg | 2.01 | 1.81 |
| 0.05    | YOLOv11-ablation | 4.87 | 4.12 |
| 0.10    | YOLOv11-ablation | 8.63 | 7.44 |
| 0.20    | — | 19.25 (mean) | 9.92 |

Mean diameter RMSE at 0.20m GSD is 19.25m — larger than most actual tree canopies. At 0.20m resolution, the pixel grid is simply too coarse for any meaningful per-tree biometric measurement.

### Area RMSE by GSD

| GSD (m) | Best Model | Area RMSE (m²) |
|---------|------------|----------------|
| 0.03 | Hybrid-v8-SAM | 36.73 |
| 0.04 | YOLOv8l-seg | 55.32 |
| 0.05 | YOLOv11-ablation | 285.66 |
| 0.20 | YOLOv11-ablation | 1,592.23 |

The jump from 0.04m to 0.05m is the same Resolution Cliff seen in detection performance — area error increases by 5× in a single GSD step.

---

## Error Propagation

Two systematic error sources exist in this pipeline:

**Mask boundary errors.** If the segmentation mask over-extends beyond the true crown boundary (including background soil or shadow), the pixel count is inflated and area is overestimated. If the mask under-extends (missing frond tips at coarser GSD), area is underestimated. Both errors scale with GSD² — small at fine resolution, large at coarse resolution.

**Planar projection assumption.** This study uses 2D orthomosaic imagery. The formula assumes the crown surface is flat and horizontal. Real oil palm crowns are three-dimensional domed structures with fronds that curve downward. The projected 2D area underestimates the true 3D biological surface area. For plantation management this is acceptable; for precise biomass modelling, LiDAR-derived 3D measurements would be more appropriate.

---

## Notes

**How does this compare to manual measurement?** Field measurement with a tape measure has typical error of 0.3–0.5m under ideal conditions. Our best automated result (RMSE=1.42m at 0.03m GSD) is higher — but automated measurement covers thousands of trees simultaneously, which manual methods cannot scale to. The trade-off is accuracy per tree vs. coverage per flight.

**What diameter is typical for mature oil palms?** Mature palms in this dataset had crown diameters between approximately 4m and 9m, with a median around 6m. An RMSE of 2m at 0.04m GSD represents roughly 30% relative error at the median — acceptable for growth monitoring and stand inventory, but not for tree-level yield modelling.

Full Python implementation — Shoelace calculator, diameter extractor, RMSE/MAE evaluation notebook: [github.com/Sai21112000/canopy-biometry-calculator](https://github.com/Sai21112000/canopy-biometry-calculator).

---

*Part 5 of 6 in the Oil Palm AI series. Next: [B6 — A Framework for Flying Smarter](#)*
