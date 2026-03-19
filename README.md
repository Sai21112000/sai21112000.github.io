# Sai's Blog

A minimalist, zero-framework blog built on **Jekyll + GitHub Pages**. Pure HTML, CSS, and Markdown — no React, no Tailwind, no build step.

> **Live at:** [sai21112000.github.io](https://sai21112000.github.io)

## Features

| Feature | Details |
|---------|---------|
| **100% Markdown** | Posts, philosophy quotes, SOPs, and the Now page — all `.md` files |
| **Image Lightbox** | Click any image to zoom. Drag to pan. ESC or ✕ to close |
| **Giscus Comments** | GitHub Discussions-powered comments (no backend needed) |
| **Dark / Light Toggle** | Click ◑ in the top-right. Respects system preference |
| **Sticky TOC** | Blog posts auto-generate a table of contents sidebar |
| **Reading Time** | Each post shows estimated reading time on the homepage |
| **Keyboard Navigation** | Press `j`/`k` to browse posts, `Enter` to open |
| **RSS Feed** | Subscribe at `/feed.xml` |
| **Open Graph Tags** | Rich previews when shared on social media |
| **Reading Progress Bar** | Scroll progress indicator at the top |
| **Custom 404** | Branded error page at `/404.html` |

## Pages

| Page | File | Description |
|------|------|-------------|
| Homepage | `index.md` | Featured quote, task checklist, post list, comments |
| Writing | `writing.md` | Full post archive with tags and descriptions |
| Philosophy | `philosophy.md` | Quotes and reflections |
| SOPs | `sops.md` | Proposals, MVPs, and future ideas |
| Now | `now.md` | What I'm currently focused on |

## Project Structure

```
├── index.md                 ← Homepage (edit quote + tasks in YAML)
├── writing.md               ← All posts archive
├── philosophy.md            ← Quotes & notes
├── sops.md                  ← Proposals / Ideas
├── now.md                   ← /now page
├── 404.md                   ← Custom 404
├── feed.xml                 ← RSS feed
├── _config.yml              ← Jekyll config
├── _posts/                  ← Blog posts (YYYY-MM-DD-slug.md)
├── _layouts/
│   ├── default.html         ← Base layout (theme toggle, lightbox, progress bar)
│   ├── home.html            ← Homepage layout (header, cards, comments)
│   ├── post.html            ← Blog post layout (TOC, reading time, tags)
│   ├── page.html            ← Generic page layout
│   └── sops.html            ← SOPs page layout
├── assets/
│   ├── style.css            ← All styling (CSS variables, dark mode)
│   ├── images/              ← Post images
│   └── pdfs/                ← PDF attachments for SOPs
├── OBSIDIAN-GUIDE.md        ← Cheat sheet for Obsidian → Jekyll workflow
├── LICENSE                  ← MIT License
└── README.md                ← This file
```

## Quick Start

### Fork & Deploy

1. **Fork** this repo
2. Go to **Settings → Pages → Source → Deploy from branch `main`**
3. Your site is live at `https://<username>.github.io`

### Customize

Edit `_config.yml`:
```yaml
title: Your Blog Name
description: Your description
url: "https://<username>.github.io"
```

Edit `_layouts/home.html` to update:
- Your name and social links
- Giscus repo ID and category ID (get yours at [giscus.app](https://giscus.app))

### Write a Post

Create `_posts/YYYY-MM-DD-your-title.md`:
```yaml
---
layout: post
title: "Your Post Title"
description: "A one-line summary."
tags: [tag1, tag2]
---

Your Markdown content here.
```

### Update the Homepage

Edit `index.md` YAML frontmatter:
```yaml
featured_quote: "Your quote"
featured_quote_source: "— Source"
tasks:
  - text: "Your task"
    done: false
```

## Obsidian Workflow

See [OBSIDIAN-GUIDE.md](OBSIDIAN-GUIDE.md) for the full cheat sheet on copying notes from Obsidian to this blog, including:
- Post templates and frontmatter
- Image path conversion (`![[]]` → `![]()`)
- Callout and footnote syntax
- Common mistakes to avoid

## Tech Stack

- **Jekyll** — Static site generator (runs on GitHub Pages)
- **Kramdown** — Markdown engine with footnotes support
- **Vanilla CSS** — No framework, CSS variables for theming
- **Giscus** — Comments via GitHub Discussions
- **Zero JavaScript frameworks** — Only minimal vanilla JS for lightbox, theme toggle, and keyboard nav

## License

[MIT](LICENSE) — Fork it, customize it, make it yours.
