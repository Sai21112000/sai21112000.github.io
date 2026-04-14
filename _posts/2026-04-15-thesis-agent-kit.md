---
layout: post
title: "I Built 9 AI Agents to Write My Thesis — Here's How It Works"
description: "A first-time thesis writer, not a strong coder, built an open-source AI agent system that helped him publish in a Q1 journal and defend his thesis. This is the full story."
tags: [ai, thesis, research, agents, academic-writing, open-source]
date: 2026-04-15 03:15:00 +0700
---

*A first-time thesis writer, not a strong coder, built an open-source AI agent system that helped him publish in a Q1 journal and defend his thesis. This is the full story.*

---

## The Problem Nobody Talks About

Every year, thousands of graduate students enter their thesis stage and hit the same wall.

They spend months reading 50-100 papers. They draft proposals, get critiqued, rewrite, get critiqued again. The back-and-forth between student and advisor consumes more time than the actual research. Communication gaps, misaligned expectations, and unclear standards lead to frustration on both sides — and in some cases, to degree withdrawals and delays that devastate students and their families.

I was one of those students. Working on oil palm instance segmentation at the Asian Institute of Technology (AIT), I was writing a thesis for the first time in my life. I was not a strong coder. I had ideas, but no clear path to turn them into a defensible, publishable document.

So I turned to AI.

---

## When AI Made Things Worse Before Making Them Better

I started experimenting with ChatGPT, Claude, Perplexity, and other LLMs to help plan and write my thesis. The results were mixed at best.

**Context length was a constant problem.** My thesis involved YOLO models, PyTorch code, datasets, and literature — far more than early LLMs could handle in a single conversation. Even when Google later introduced long-context models (128K to 1M tokens), processing quality degraded with large inputs.

**Hallucination was the real danger.** LLMs would fabricate citations that looked perfectly real. They would invent metrics I never measured. They would generate plausible-sounding paragraphs that traced to no research question. When I cross-checked outputs across different models, I got different narratives, different methodologies, and contradictory claims.

**Multi-agent collaboration was even worse.** When I tried using multiple agents together — one for literature review, one for writing, one for review — they hallucinated *more*, not less. With too much shared context, agents would extract minimal knowledge from the knowledge base or fail to follow orchestration plans entirely.

I realized: the problem was not that AI could not help with thesis writing. The problem was that AI had no constraints, no quality floor, and no accountability structure.

---

## The Breakthrough: Skills Before "Skills" Existed

Long before AI platforms formally introduced agent skills, I started writing my own.

I went to my university website, scraped data about my advisor — [Dr. Sarawut Ninsawat](https://ait.ac.th/people/dr-sarawut-ninsawat/) — downloaded his CV, and gathered around ten of his published papers. I fed these into ChatGPT, Claude, and Perplexity with a specific mission: extract patterns. Writing style. Paragraph structure. Formal tone. Citation habits. Section organization.

Each model produced structured `.md` files. I merged them into a single `master.md` that captured my advisor's preferences and my university's standards. This file became the foundation for creating **agent skills** — specialized instruction sets that governed how each AI agent should behave.

From this, the first agents were born:
- A **writer** that followed extracted Q1 paragraph patterns
- A **reviewer** that critiqued drafts against academic standards
- A **citation checker** that refused to let fabricated references through

Over 6-8 months, iterating through my actual thesis work, I refined and expanded these into what became **Thesis Agent Kit**.

---

## What Thesis Agent Kit Is

Thesis Agent Kit is an open-source system of **9 specialist AI agents**, **6 immutable writing laws**, and a **3-layer architecture** that turns any AI code editor into a research writing command center.

It does not write your thesis for you. It enforces the standards that get theses defended and papers published.

### The 9 Agents

| Agent | What It Does |
|-------|-------------|
| **Research Interrogator** | Adversarial onboarding. Builds your project config from scratch. Scores every section 0-100%. Blocks writing until 90%+ aggregate confidence. |
| **Research Writer** | Drafts Q1-quality prose following T-C-E-L paragraph architecture. Domain-aware. Section-by-section with quality gates. |
| **Research Advisor** | Strategic guidance on research questions, research design, and defense preparation. |
| **Research Reviewer** | Adversarial 6-pillar critique: Novelty, Methodology, Gap Integrity, Result Validity, Writing Quality, Citation Integrity. |
| **Citation Checker** | 5-step verification via CrossRef DOI chain. Every reference gets a verdict: VERIFIED, FLAGGED, or FABRICATED. |
| **Literature Review** | PRISMA-informed search and thematic synthesis with gap formula enforcement. |
| **LaTeX Formatting** | Venue-specific templates, figure/table environments, BibTeX/biber, compilation fixes. |
| **Figure Generation** | Code-first for data plots (Matplotlib/TikZ). AI imagery for concepts only. Full RESULTS/ traceability. |
| **Snapshot Manager** | Version history without Git. Save, restore, diff, and browse any file state. |

Each agent was born from a real pain point encountered during the thesis process. None were designed theoretically — they were extracted from practice.

### The 6 Immutable Laws

Every paragraph produced by any agent must follow these rules. No override. No exceptions.

1. **RQ Traceability** — Every paragraph traces to a Research Question. Orphan paragraphs are deleted.
2. **No Fluff** — Forbidden phrases like "it is important to note" or "in today's world" are rejected automatically.
3. **Register** — No contractions. Correct tense per section. No vague quantifiers.
4. **Citation Integrity** — Every factual claim needs a citation or gets flagged `[CITATION NEEDED]`. No fabricated references.
5. **Voice Discipline** — Passive in Methods/Results. Active declarative topic sentences.
6. **T-C-E-L Architecture** — Every body paragraph follows Topic, Cite, Explain, Link. Every time.

These laws exist because flexible prompting failed. Without hard constraints, LLMs consistently produced fluff, mixed tenses, orphaned paragraphs, and fabricated citations. The laws encode the minimum quality floor that got the thesis defended and the Q1 paper accepted.

### The 3-Layer Architecture

| Layer | Contents | Who Maintains It |
|-------|----------|-----------------|
| **Core Layer** | 6 Laws, IMRaD structure, 6-pillar QA rubric | Kit maintainers (domain-agnostic, never edited per project) |
| **Domain Layer** | Terminology banks, metrics, forbidden phrases, evaluation norms | Community (add new domains by copying the template) |
| **Project Layer** | `PROJECT-CONFIG.md` — your title, RQs, methods, data, results | You (fill once, all agents read it) |

This separation was the key insight for solving multi-agent hallucination. By grounding every agent in explicit, layered context — rather than free-form conversation — agents know exactly what they can reference and what they must flag as unknown.

---

## The Confidence Gate

Before any writing begins, the Research Interrogator validates your research foundations through a structured interview. Every section of your project config is scored 0-100%, weighted by importance:

- Research Questions: 20% weight
- Research Problem: 15%
- Research Gap: 15%
- Method Mapping: 10%
- Hypotheses: 10%
- Identity and Domain: 10%
- Scope: 5%
- Aim: 5%
- Contributions: 5%
- Experiment Data: 5%

The aggregate score determines what happens next:

| Score | Verdict |
|-------|---------|
| 90%+ | READY — full drafting unlocked |
| 75-89% | CONDITIONAL — Chapters 1-2 only |
| 50-74% | NOT READY — fix weak sections first |
| Below 50% | BLOCKED — fundamental rethink needed |

The 90% threshold was calibrated through experience. Configs scoring 75-89% consistently produced drafts that needed major rewrites. Configs at 90%+ produced drafts that only needed polishing.

---

## How It Works in Practice

```
Step 1: /interview     → Build and validate your project config (90%+ gate)
Step 2: /write-chapter → Draft section-by-section with quality gates
Step 3: /review-draft  → Adversarial 6-pillar critique until STRONG verdict
Step 4: /figures       → Generate publication-ready plots and diagrams
Step 5: /check-citations → Verify every reference via CrossRef DOI chain
→ LaTeX formatting auto-applied → PDF → Submit or defend
```

The kit works with **Cursor**, **Windsurf**, **ChatGPT** (Custom GPT), **Claude** (Projects), or any LLM that accepts system prompts.

---

## What Broke Along the Way

Building this over 6 months was not smooth. Here are the honest failures:

**Multi-agent hallucination.** When agents shared context, they invented data, lost track of which RQ a section served, and generated plausible but fabricated citations. The solution: the "refuse to invent" principle. Every metric must trace to the `RESULTS/` folder or get flagged. Every claim needs a citation or gets flagged. Agents that cannot verify something must say so.

**Flexible prompting failed.** Early versions used suggestions instead of rules. LLMs ignored suggestions under pressure (long context, complex sections). Making the 6 Laws immutable — no agent can override them — was the fix.

**The snapshot system was over-engineered.** Building a custom version control system made sense for Git-unfamiliar students, but in hindsight, a thin Git wrapper would have been simpler and more transferable.

---

## Results

This system was developed in collaboration with my advisor, [Dr. Sarawut Ninsawat](https://ait.ac.th/people/dr-sarawut-ninsawat/) at the [Asian Institute of Technology](https://ait.ac.th/). Over 6-8 months of iterative development during my actual thesis work:

- **Q1 journal paper accepted** — written using the same agent skills
- **Thesis successfully defended**
- **Tested with 4 additional students** at AIT across different subdomains and fields
- **Open-sourced** as Thesis Agent Kit v2.1 with 9 agents, 6 domains, and full documentation

I want to acknowledge and thank Dr. Sarawut Ninsawat for his collaboration and guidance throughout this project. We are both grateful to AIT for supporting this work and for the opportunity to share these tools with the research community.

---

## How to Get Started

**For Cursor / Windsurf users:**
```bash
git clone https://github.com/Sai21112000/Thesis-Agent-Kit.git
cp -r .agent/ /path/to/your/thesis/
cp .cursorrules /path/to/your/thesis/
cp PROJECT-CONFIG.md /path/to/your/thesis/
# Then type: /interview
```

**For ChatGPT / Claude web users:**
Paste `custom-gpt/KNOWLEDGE-BASE.md` into your system instructions. All 9 agents work there too.

**Try the companion Custom GPT:**
[Sai's Prompt Architect](https://chatgpt.com/g/g-6989c032c22481918f4b6120af3e222d-sai-s-prompt-architect)

---

## What's Next

I am releasing this toolkit as an open-source portfolio project. I am not planning further development from my side, but the project is fully open to contributions. I would love to see:

- New domain modules (economics, chemistry, education, law, engineering)
- Venue-specific LaTeX templates
- Translations of the knowledge base
- Forks adapted for conference papers, dissertations, or grant proposals

If this helps even one student avoid the frustration I went through, it was worth building.

---

## Links

- **Thesis Agent Kit Repository:** [github.com/Sai21112000/Thesis-Agent-Kit](https://github.com/Sai21112000/Thesis-Agent-Kit)
- **Oil Palm Segmentation Thesis (source code):** [github.com/Sai21112000/oil-palm-instance-segmentation](https://github.com/Sai21112000/oil-palm-instance-segmentation)
- **Custom GPT:** [Sai's Prompt Architect](https://chatgpt.com/g/g-6989c032c22481918f4b6120af3e222d-sai-s-prompt-architect)
- **YouTube Demo:** [Watch on YouTube](https://www.youtube.com/watch?v=ck-oeeHJqNA)
- **Advisor:** [Dr. Sarawut Ninsawat — AIT](https://ait.ac.th/people/dr-sarawut-ninsawat/) | [Google Scholar](https://scholar.google.com/citations?user=xkASpL4AAAAJ&hl=en)
- **University:** [Asian Institute of Technology](https://ait.ac.th/) | [AIT LinkedIn](https://www.linkedin.com/school/ait-asia/)

---

*9 agents. 6 laws. One thesis, defended. Stop procrastinating — type `/interview`.*
