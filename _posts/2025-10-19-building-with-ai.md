---
layout: post
title: "How I Built This Blog in One Session — With AI as My Co-Pilot"
description: "From zero to a fully-featured minimalist blog using Jekyll, GitHub Pages, and AI pair programming. A transparent look at ethical AI-assisted development."
tags: [ai, webdev, productivity, open-source]
date: 2025-10-19 12:00:00 +0700
last_modified: 2025-10-19
---

I built this entire blog — from the first `git init` to a fully-featured, open-source, dark-mode-enabled, comment-ready static site — in a single working session. Not because I'm fast. Because I had help.

This post is a transparent account of how AI tools helped me learn, build, and ship at a speed I didn't think was possible. And why I think crediting your tools honestly is the most important thing a developer can do in 2026.

## The Starting Point

I had an idea: a minimalist, typography-first blog. No React. No Tailwind. No frameworks. Just pure HTML, CSS, and Markdown — hosted free on GitHub Pages.

I knew *what* I wanted. I didn't know *how* to wire Jekyll layouts, Liquid templating, CSS variables for dark mode, or giscus comment integration. Not from memory, anyway.

So I opened my editor and started a conversation with an AI assistant.

## What We Built Together

Here's what exists now, feature by feature:

| Feature | How AI Helped |
|---------|---------------|
| **Jekyll site structure** | Generated `_layouts/`, `_config.yml`, and explained how Liquid templating works |
| **Dark/Light mode** | Wrote the CSS variable system and the toggle JS — I learned how `prefers-color-scheme` works |
| **Image lightbox with drag-to-pan** | Built the entire lightbox from scratch — click to zoom, drag to pan, ESC to close |
| **Sticky Table of Contents** | Auto-generated from `<h2>`/`<h3>` headings with scroll-based highlighting |
| **Giscus comments** | Guided me through installing the GitHub App, configuring the script tag, and choosing the right mapping |
| **Reading time badges** | One Liquid filter: `post.content \| number_of_words \| divided_by: 200` |
| **Keyboard navigation** | `j`/`k` to browse posts, `Enter` to open — vim-style, built in vanilla JS |
| **RSS feed** | Generated `feed.xml` with proper Liquid loops |
| **Open Graph meta tags** | SEO-ready social sharing previews |
| **Homepage cards** | Dynamic quote from philosophy page + task checklist — both editable from YAML |
| **/now page** | Inspired by [nownownow.com](https://nownownow.com) — what I'm focused on right now |
| **Obsidian workflow guide** | A full cheat sheet for copying notes from Obsidian to Jekyll |

## The Tools I Used

I want to be explicit about this:

- **Google Gemini (Antigravity Agent)** — Primary AI pair programmer. Wrote code, debugged issues, suggested features I hadn't thought of
- **GitHub Copilot** — Inline code suggestions while editing
- **ChatGPT** — Research and brainstorming for content ideas
- **Obsidian** — Writing environment for drafting posts in Markdown

The AI didn't build this *for* me. It built this *with* me. There's a difference.

## What I Actually Learned

This is the part people skip when they talk about AI-assisted coding. Here's what I *genuinely understood better* after this session:

1. **How Jekyll processes Markdown** — I now understand the `layout → content` chain, YAML frontmatter, and Liquid filters
2. **CSS Custom Properties** — How `var(--bg)` enables theme switching without JS class toggles
3. **GitHub Discussions API** — How giscus maps page pathnames to discussion threads
4. **Responsive design without frameworks** — Media queries, flexbox, `max-width` constraints
5. **Git workflow** — Staging, committing, pushing, and why future-dated filenames break Jekyll

I could rebuild this from scratch now. That's the test.

## The Ethical Argument

There's a growing debate about whether AI-generated code is "real" work. Here's my take:

> **Using AI without understanding is copy-pasting from Stack Overflow with extra steps. Using AI while actively learning is the most efficient education system ever built.**

I asked questions. I understood the answers. I made design decisions. The AI executed faster than I could type — but the *direction* was always mine.

### Credit Where It's Due

Every architect credits their tools. Every filmmaker credits their camera operator. Every researcher cites their sources.

**If AI helped you build something, say so.** It doesn't diminish your work — it shows intellectual honesty and gives others permission to learn the same way.

## The Productivity Multiplier

Some rough numbers from this session:

| Metric | Estimate |
|--------|----------|
| **Working session** | ~6 hours |
| **Total features shipped** | 15+ |
| **Files created/modified** | 20+ |
| **Lines of CSS** | ~700 |
| **Lines of HTML/Liquid** | ~500 |
| **External dependencies** | 0 (excluding giscus) |
| **JavaScript frameworks** | 0 |
| **Cost** | $0 (GitHub Pages is free) |

Without AI assistance, this would have taken me a full weekend of documentation-reading and trial-and-error. With AI, it took one focused session.

That's not cheating. That's productivity. 
Here is my tutorial: [Watch on YouTube](https://www.youtube.com/watch?v=RyhJJnfNMDI)

## What's Next

- More blog posts (obviously)
- Client-side search
- An Obsidian-to-Jekyll auto-sync script
- Maybe a custom favicon

The entire source code is [open source on GitHub](https://github.com/Sai21112000/sai21112000.github.io). Fork it, customize it, make it yours.

---

*This post was written by me, with AI assistance for code examples and structure suggestions. The ideas, opinions, and experiences are entirely my own.*
