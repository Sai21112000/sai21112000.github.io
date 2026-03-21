---
layout: post
title: "How I Used GIS to Map a Pandemic: Lessons from Spatial Epidemiology"
date: 2024-12-08
last_modified_at: 2026-03-21
tags: [gis, remote-sensing, spatial-analysis, covid-19, hotspot-mapping, arima, ait, research]
---

In December 2024, I submitted a special study as part of my Master's in Remote Sensing and GIS at the Asian Institute of Technology. The topic: using Geographic Information Systems to understand how COVID-19 spread — and why geography mattered more than most people realized.

This is a walkthrough of what I reviewed, what surprised me, and why spatial thinking is one of the most underrated skills in data science today.

## Why Geography Matters in a Pandemic

When COVID-19 broke out, most early analyses were temporal — how many cases today vs. yesterday, which country's curve was flattening. But the *spatial* dimension was just as critical: **where** were cases clustering? **Which districts** were hotspots? **How fast** was the virus moving across geography?

GIS — Geographic Information Systems — gives you the tools to answer those questions. And during the pandemic, it became one of the most important analytical lenses governments and health organizations had.

## What the Research Actually Shows

My study reviewed two major analytical paradigms applied to COVID-19 data:

### 1. Trend Analysis Methods
I surveyed four approaches researchers used to model and forecast case trajectories:

- **ARIMA (Auto-Regressive Integrated Moving Average):** The statistical workhorse. Applied by Rajan Gupta (2021) to Indian state-level data, ARIMA(1,1,2) models predicted that without intervention, cases could reach 80 million within 30 days. The math relies on stationarity testing (Dickey-Fuller), rolling statistics, and lagged error terms.

- **Successive Approximation:** Qasim et al. (2020) used real-time global data and mean ratios (η) to bound future case counts. Their model predicted worldwide lower-bound cases of 247,007 and upper-bound of 1,667,719 from March 2020 — and they were right.

- **Google Trend Analysis:** Pan et al. (2020) found that Google search queries correlated directly with confirmed case counts. Deep learning models using Google Trends data outperformed traditional statistical models in prediction accuracy.

- **Continental Analysis via Microsoft Excel:** Liu et al. (2022) analyzed 490 million confirmed cases across six continents, finding Europe had the highest incidence rate and South America the highest death rate per million.

### 2. Hotspot Mapping Methods

Trend analysis tells you *when*. Hotspot mapping tells you *where*.

- **Getis-Ord Gi\* Statistic:** The core spatial tool. It generates z-scores and p-values per geographic unit. High z-score + low p-value = statistically significant hotspot. Parvin et al. (2021) used this to map COVID-19 hotspots at the district level across India — producing maps that directly informed resource allocation.

- **Gaussian Mixture Models (GMM):** ML-based clustering. Tested at 10, 50, and 1,500 cluster levels, GMM visualized how hotspot density evolved between India's first and second COVID waves.

- **Kernel Density Estimation (KDE):** Spacetime KDE extended traditional KDE into the temporal dimension, enabling 4D visualization of case density across both geography and time.

- **Open Source Data Integration:** IBM South Africa built real-time hotspot visualizations using open datasets — demonstrating that high-resolution outbreak mapping doesn't require government data monopolies.

## The Bigger Takeaway

What struck me most in this review wasn't any single method — it was the realization that **every COVID-19 policy decision had a spatial component that data science often missed.**

Which districts get the ICU beds? Which neighborhoods get the testing centers first? Which borders need the tightest controls? These are fundamentally geographic questions. And yet, in much of the early pandemic response, they were treated as spreadsheet problems.

GIS bridges that gap. It turns spatial data into decision intelligence.

## Why This Connects to What I Do Now

This study happened during my Master's in RS-GIS, just before I moved full-time into my deep learning thesis on UAV imagery. At first glance, COVID hotspot mapping and oil palm canopy detection seem unrelated.

But the underlying logic is identical:
- Both ask: *where is this thing concentrated?*
- Both use spatial statistics to answer with confidence intervals
- Both convert pixels or coordinates into actionable geographic intelligence

The tools differ. The thinking is the same.

Full Paper: [Special Study](/assets/pdfs/Special Study.pdf) 
or [Paper](https://sai21112000.github.io/sops.html)
---

*Special Study submitted December 2024 | M.Eng in Remote Sensing and GIS | Asian Institute of Technology*  
*Supervised by Prof. Nitin Kumar Tripathi | Committee: Dr. Chitrini Mozumder*  
*Supported by AIT Fellowship Scholarship*
