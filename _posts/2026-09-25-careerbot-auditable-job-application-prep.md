---
layout: post
title: "CareerBot: prepare auditable job applications without auto-apply"
description: "CareerBot is an open-source MCP toolkit that scores jobs against a Master CV, drafts checked resumes, and saves SHA-256 snapshots. It does not scrape boards or submit applications."
tags: [CareerBot, MCP, agents, open-source]
date: 2026-09-25 02:30:00 +0700
---

CareerBot is an open-source Python toolkit that scores job fit, drafts verified application artifacts, saves immutable snapshots, and tracks what you prepared. It does not scrape job boards, and it does not submit applications for you.

That boundary is the product. If you want a bot that clicks Apply, this is the wrong repository. If you want a local workflow that shows its evidence and stops at legal, identity, and platform gates, it is meant for that.

The project is alpha, MIT licensed, and available at [github.com/Sai21112000/CareerBot](https://github.com/Sai21112000/CareerBot). A short public overview lives at [sai21112000.github.io/CareerBot](https://sai21112000.github.io/CareerBot/). The argument for refusing to auto-apply is also on Medium: [The useful AI job agent is the one that refuses to apply](https://medium.com/@vaidyasaiteja143/the-useful-ai-job-agent-is-the-one-that-refuses-to-apply-046e8e664843).

## What problem it is for

Applying well is repetitive and easy to falsify by accident. You collect postings, judge fit, rewrite a resume toward the role, export a PDF, and try to remember what you sent. A language model can speed up the rewriting. It can also imply experience that is not in the source CV, because overlap reads like a better draft.

CareerBot keeps those steps separate and inspectable:

- Rank and deduplicate jobs you supply.
- Score them against a private Master CV.
- Draft positioning, a LaTeX resume, and a cover letter from that evidence.
- Check ATS hygiene, claim provenance, and one-page PDF output.
- Save a snapshot with SHA-256 hashes.
- Record state locally, with optional Airtable tracking.
- Hand off instead of submitting when the next step is not allowed.

## How the pieces fit

You can use CareerBot as a CLI, as a local MCP server inside Cursor, or as an authenticated remote MCP endpoint you host yourself. MCP (Model Context Protocol) is the interface that lets a client call a fixed set of tools instead of giving a model an open-ended browser.

The runtime is a small set of modules:

- **Discovery** normalizes and ranks caller-supplied jobs. It does not include LinkedIn, JobsDB, or JobThai scrapers.
- **Scoring** uses xAI's Responses API and the Master CV text. The CV is the source of experience. Strategy notes can prioritize roles. They cannot create new history.
- **Tailoring and validation** draft LaTeX and then apply deterministic checks. Tectonic compiles the PDF locally.
- **Snapshots** store the package and a manifest. Verification recomputes hashes.
- **Policy** decides whether an authorized connector exists. If it does not, or if the step needs CAPTCHA, MFA, consent, or a legal declaration, the result is a manual handoff.
- **Tracking** writes an idempotent local record. Airtable is optional.

Twelve tools are exposed: `consult_career_positioning`, `save_career_strategy`, `get_career_strategy`, `search_jobs`, `score_job`, `tailor_resume`, `generate_cover_letter`, `save_application_snapshot`, `verify_snapshot`, `track_application`, `submission_hard_stops`, and `assess_submission`.

Private files stay local. The repository ignores `.env`, the real Master CV, resume PDFs, the application log, strategy snapshots, and active platform queues. Public examples use placeholders.

## The workflow

1. **Supply** jobs manually, from a parsed alert, or from a permitted adapter.
2. **Rank** and deduplicate that set. The CLI `triage` command does this locally, without a model call.
3. **Score** fit with the Master CV. Model scoring is available through `score_job` and the `apply` path.
4. **Prepare** a resume and cover letter from verified claims.
5. **Validate** ATS checks, claim provenance, and a one-page PDF.
6. **Snapshot and track** the package. The manifest is how you later prove which files were saved.
7. **Policy gate.** An authorized, qualified connector may proceed. Anything blocked, ambiguous, or unsupported becomes a handoff.

There is no built-in job-portal submit connector. `apply` creates a local snapshot and CSV row. It does not send the application.

## Install and first run

Requirements: Python 3.12+, [uv](https://docs.astral.sh/uv/), and [Tectonic](https://tectonic-typesetting.github.io/) if you want PDFs.

```bash
git clone https://github.com/Sai21112000/CareerBot.git
cd CareerBot
uv sync --extra dev
cp .env.example .env
cp AGENTS.md.example AGENTS.md
cp data/master_resume.tex.example data/master_resume.tex
chmod 600 .env data/master_resume.tex AGENTS.md
uv run careerbot --help
```

Replace every `[VERIFIED ...]` marker in the resume template with text you can support. Put source `.tex`, `.txt`, or `.md` files in the ignored `data/master_cv/` directory. Model-backed commands need `CAREERBOT_XAI_API_KEY`. Optional Airtable tracking needs a personal access token and base ID in `.env`, not in the repo.

A safe first command uses the fictional sample:

```bash
uv run careerbot triage --input examples/jobs.json --max-jobs 10
```

Set conservative limits before you point it at real inputs:

```dotenv
CAREERBOT_MAX_JOBS_PER_RUN=20
CAREERBOT_MAX_APPLICATIONS_PER_RUN=5
CAREERBOT_MAX_RUN_MINUTES=30
```

## Trade-offs

| You get | You do not get |
|---|---|
| A scored, evidence-backed draft | A scraper for major job boards |
| A one-page PDF check | A guarantee that every employer ATS will parse it |
| An immutable snapshot | Automatic submission |
| Optional Airtable tracking | Full remote-to-local reconciliation in the CLI |
| Scoped MCP tools | An agent with permission to pass CAPTCHA or MFA |
| Local private data | A hosted multi-tenant product |

`dry_run`, `review`, and `auto` name an operator contract. They are not yet a single authorization layer around every local write. Treat `auto` as unavailable until you add and qualify your own connector. In this alpha, it still does not submit to a portal.

Google Gmail and Calendar adapters exist as optional libraries. They are not wired into a complete follow-up command. `follow-up sync` lists due local records. `reconcile` reports configuration readiness.

## Limits to know before you clone

- You must bring the jobs. Discovery does not browse the web for you.
- The model can draft. Deterministic checks reduce unsupported claims. They do not replace reading the PDF.
- Hypothetical portfolio work must be labeled proposed, planned, or in progress.
- Remote MCP needs your own HTTPS proxy and a real token. The sample static bearer check is for development. Production should use OAuth or JWT.
- Start with `examples/` and development credentials.

## What to do next

Clone the repository, run `triage` on the example file, and read a snapshot manifest before you connect Airtable or a model key. If the workflow you want is "log in and apply until the queue is empty," stop here. CareerBot will not do that step for you.

If the workflow you want is "show the fit, show the claims, save the bytes, and stop," the CLI help and the MCP tool list are the next page.

## FAQ

### Does CareerBot apply to jobs for me?

No. No shipped command submits to a job portal. Unsupported or restricted steps return a manual handoff.

### Does it scrape LinkedIn or JobsDB?

No. Jobs have to be supplied by you, by a parsed alert, or by an adapter you are allowed to use.

### What is the Master CV for?

It is the factual boundary. Scoring and tailoring are supposed to use that text, not invent employment, clients, metrics, or finished projects.

### Can I track applications in Airtable?

Yes, as an optional integration with a least-privilege token. The local CSV remains the backup ledger. CLI reconciliation does not yet fully sync remote and local state.
