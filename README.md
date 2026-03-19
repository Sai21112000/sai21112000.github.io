# Sai's Blog

A minimalist, Markdown-driven blog built on GitHub Pages (Jekyll). Write in Obsidian, push to GitHub, and your site updates automatically.

## Features
- **100% Markdown** — Write posts, philosophy quotes, and SOPs entirely in `.md`
- **RSS Feed** — Subscribers get updates at `/feed.xml`
- **Dark/Light Toggle** — Click the ◐ button in the top-right corner
- **Reading Progress Bar** — A thin red bar at the top tracks your scroll progress
- **Sticky Table of Contents** — Blog posts auto-generate a TOC sidebar on wide screens
- **Open Graph Tags** — Links shared on LinkedIn/Twitter/WhatsApp show rich previews
- **Custom 404 Page** — Broken links show a branded error page

## Site Structure

```
├── index.md              ← Homepage (edit projects list here)
├── philosophy.md         ← Quotes & notes page
├── sops.md               ← Proposals / MVPs / Ideas
├── 404.md                ← Custom 404 page
├── feed.xml              ← RSS feed (auto-generated)
├── _posts/               ← Blog posts (Markdown)
│   └── 2026-03-19-first-post.md
├── _layouts/             ← HTML templates (don't touch unless customizing)
├── assets/
│   ├── style.css         ← All styling
│   ├── images/           ← Drop images here
│   └── pdfs/             ← Drop PDFs here for SOPs
└── _config.yml           ← Site configuration
```

## How To

### Add a Blog Post
1. Create a file in `_posts/` named `YYYY-MM-DD-title.md`
2. Add frontmatter at the top:
   ```yaml
   ---
   layout: post
   title: "Your Title"
   tags: [tag1, tag2]
   last_modified: 2026-04-01    # optional
   ---
   ```
3. Write your content in Markdown below the `---`

### Add a Quote to Philosophy
Open `philosophy.md` and add:
```markdown
> "Your quote here."

Your notes about this quote.
```

### Add an SOP / Proposal
Open `sops.md` and add a bullet under the appropriate section:
```markdown
- **Project Name** — Description. [Download PDF](/assets/pdfs/file.pdf)
  *Status: Idea*
```

### Add Images
Drop images into `assets/images/` and reference them:
```markdown
![Description](/assets/images/photo.jpg)
```

### Update Selected Work
Open `index.md` and edit the `projects:` list in the YAML frontmatter.

## Deployment
Push to `main` and GitHub Pages builds automatically:
```bash
git add .
git commit -m "new post"
git push origin main
```
