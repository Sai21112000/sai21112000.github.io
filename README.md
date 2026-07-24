# Sai Teja Vaidya

Personal research site and long-form technical blog, built with Jekyll for GitHub Pages.

**Live:** [sai21112000.github.io](https://sai21112000.github.io)

## Site features

- Feed-first editorial homepage with real post images or tag-based visual fallbacks
- Responsive navigation, persisted dark mode, reading progress, image lightbox, and `j`/`k` keyboard browsing
- Long-form post layout with generated desktop TOC, author context, related essays, Giscus discussion, and sequence navigation
- Filterable essay archive and a six-part UAV thesis sequence
- Data-driven resume and portfolio pages
- Philosophy, Now, Book a call, About, and Powerlifting pages in one shared site shell
- RSS, canonical URLs, Open Graph/Twitter metadata, and a custom 404
- Print-friendly resume styles

## Content structure

```text
_posts/                 Markdown essays (existing /posts/:title.html permalinks)
_layouts/               Page, post, archive, resume, portfolio, and specialty layouts
_includes/              Shared head, header, footer, cards, author, Giscus, and scripts
_data/resume.yml        Confirmed resume facts
_data/projects.yml      Portfolio and passion projects
sequences/              Curated reading paths
assets/style.css        Shared responsive design system
assets/images/          Portrait, post, and powerlifting media
```

## Write and publish

Create `_posts/YYYY-MM-DD-slug.md`:

```yaml
---
layout: post
title: "Post title"
description: "One-line summary."
tags: [computer-vision, research]
image: /assets/images/optional-cover.jpg
---
```

The existing Obsidian workflow remains supported. See [OBSIDIAN-GUIDE.md](OBSIDIAN-GUIDE.md) for frontmatter, image conversion, callouts, and publishing notes.

Sequence posts can add:

```yaml
series: uav-thesis
series_order: 1
```

## Local preview

Install the locked Ruby dependencies and start Jekyll:

```sh
bundle install
bundle exec jekyll serve
```

The site is available at `http://localhost:4000`. No unsupported plugins or JavaScript framework are required. GitHub Pages builds the site directly from the repository.

## License

[MIT](LICENSE)
