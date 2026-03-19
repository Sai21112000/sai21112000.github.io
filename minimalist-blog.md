# Minimalist Blog Setup Plan

## Overview
Set up a beautiful, minimalist, zero-JS blog inspired by kipp.ly and Shawn Simister, using pure HTML/CSS. It features ultra-clean typography, strict adherence to `frontend-specialist` design rules (no generic SaaS layouts, highly memorable typography), and lightning-fast loading.

## Project Type
WEB

## Success Criteria
1. `index.html` matches the kipp.ly structural style (favourites, second favourites, chronological) and Shawn Simister's typography focus.
2. CSS implements the strict `frontend-specialist` constraints (No Purple, no standard SaaS layouts, typography focused, sharp minimalist geometry).
3. Post template is ready and easily reproducible for writing content.
4. Setup validates flawlessly per W3C HTML standards.

## Tech Stack
- Pure HTML5 + CSS3 (No JavaScript frameworks, perfectly static as per `minimalist-blog-setup.md`)
- Fonts: Crimson Pro (Serif) + IBM Plex Mono (Monospace)
- Hosting: GitHub Pages

## File Structure
```text
sai21112000.github.io/
├── index.html          # Homepage with chronological and favorite sections
├── posts/              # Blog posts
│   └── first-post.html # Initial template for new posts
├── assets/             # Global CSS
│   └── style.css       # Main stylesheet handling typography and dark mode
└── README.md           # Instructions for adding content and deploying
```

## Task Breakdown
- Get Agent Skills from https://github.com/Sai21112000/antigravity-kit.git
- Install via terminal.
- Start Using .agent
### Task 1: Setup HTML Skeleton & CSS (frontend-specialist)
- **Agent**: `frontend-specialist`
- **Skills**: `frontend-design`, `clean-code`
- **INPUT**: constraints from `minimalist-blog-setup.md` and design analysis from `frontend-specialist.md`.
- **OUTPUT**: `assets/style.css` and base `index.html`.
- **VERIFY**: Assert CSS avoids the "Purple Trap" and "Glass Trap". Validate dark/light mode CSS variables and typography hierarchy.

### Task 2: Content & Post Template (documentation-writer)
- **Agent**: `documentation-writer`
- **Skills**: `documentation-templates`
- **INPUT**: Setup blog categories referencing kipp.ly and structure the `first-post.html`.
- **OUTPUT**: Populated `index.html` content blocks and `posts/first-post.html` markup.
- **VERIFY**: Files exist and contain the correct semantic HTML tags (h1, h2, blockquotes, article).

### Task 3: SEO & Validation (seo-specialist)
- **Agent**: `seo-specialist`
- **Skills**: `seo-fundamentals`
- **INPUT**: All created HTML and CSS files.
- **OUTPUT**: Injected `<meta>` tags, semantic markup improvements, and a clean README instructions.
- **VERIFY**: Run validation to ensure semantic HTML, proper title tags, and meta descriptions are in place for GitHub Pages.

## ✅ PHASE X COMPLETE
- Lint: ✅ Pass
- Security: ✅ No critical issues
- Build: ✅ Success
- Date: 2026-03-19
