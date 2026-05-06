---
layout: post
title: "Dopamine.Diet: A Local-First Productivity System for Serial Project Switchers"
description: "A local-first deep-work dashboard for people who keep switching projects before anything ships."
tags: [productivity, systems, vibe-coding, local-first]
date: 2026-05-06 10:58:00 +0700
---

I chase dopamine quietly: a new tab for a paper I will not finish, a shinier repository, a third Docker container before the second model trains. I stay busy, but ship nothing. The portfolio becomes fiction.

The brain learns that scrolling and notifications release dopamine faster than training a model or writing a README. Deep work feels expensive; context switching feels productive. But every switch resets working memory. After eight hours, you are exhausted and nothing is finished.

So I built **Dopamine.Diet**: a local-first productivity system for serial project switchers.

---

## The Rule

Earn dopamine through completion, not consumption.

Dopamine.Diet uses the rewards I already like - progress bars, streaks, checkboxes, timers, visual feedback - and points them at deep work instead of novelty hunting.

The app is not trying to make productivity cute. It is trying to make avoidance visible.

---

## How It Works

The core is **Combo Mode**, a two-part dashboard.

**Page 1: Weekly Command Center.** Pick exactly two flagship projects. Everything else goes into a **Parking Lot**: a holding cell for ideas that cannot become new tabs today. Define one **Learning Sprint**. No more.

**Page 2: Daily Deep-Work Tracker.** Morning priming leads into two **Deep Blocks** with a manual timer, Play/Pause/Reset controls, and six 15-minute segment checks. A **Distraction Log** captures the urge to switch without acting on it. The **Win Log** forces me to name what I shipped, not what I planned.

The **Scoreboard** auto-grades the day:

- **A**: both blocks complete, zero switching, real wins logged
- **B**: both blocks complete
- **C**: one block complete
- **D**: avoidance won

The reward stays locked until both deep blocks are complete. A GitHub-style contribution graph shows the streak, and the archive lets me look back at past weeks to see whether I actually shipped or just hallucinated progress. **Focus Mode** hides everything except the active block.

---

## The Build

I vibe-coded it in **React**, **Tailwind**, **Zustand**, and **Framer Motion**, then hosted it on **GitHub Pages**.

It is strictly **local-first**:

- `localStorage` persistence
- Markdown export for weekly and daily logs
- JSON import for restoring saved state
- no backend
- no auth

Your data stays in the browser unless you export it.

The sound design is intentionally small: a Web Audio tick on checkbox interactions and a three-note chime when a timer completes. The point is feedback, not gamification overload.

---

## The Project Menu

The first version had my personal stack hardcoded into the app: agents, research papers, Docker, LLM deployment, competitions. That worked for me, but it made the project feel like a private script.

The current version keeps the defaults, but makes them editable. Tracks can be renamed or deleted, new tracks can be added, and each track has an inline field for custom items. The flagship project dropdown is fed by this living menu, so project names stay consistent instead of becoming five slightly different versions of the same idea.

That change matters because the app is supposed to support intentionality. If a project enters the system, I should have typed it, chosen it, and owned it.

---

## Data & Architecture

The Zustand store holds weekly metadata, flagship projects, daily blocks, distraction logs, review checks, archive entries, and the editable project menu. Changes serialize into `localStorage`, so the app works as a static site on GitHub Pages.

There is no database to configure and no account system to maintain. That constraint is the point. I wanted the smallest possible architecture that still keeps me honest.

---

## The Honest Part

The streak counter is still at `0`.

That is embarrassing, but useful. A productivity tool is not proven by its UI. It is proven when it changes behavior. I am eating my own dog food until that number moves.

> "You do not rise to the level of your goals. You fall to the level of your systems."
>
> - James Clear

---

**Try it:** [sai21112000.github.io/Dopamine.Diet](https://sai21112000.github.io/Dopamine.Diet/)

**Source:** [github.com/Sai21112000/Dopamine.Diet](https://github.com/Sai21112000/Dopamine.Diet)
