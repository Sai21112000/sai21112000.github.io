# YouTube Tutorial Script
**Title:** "I Built a Blog in One Session with AI — Here's How (Open Source)"
**Duration:** ~8-10 minutes
**Tone:** Casual, educational, honest

---

## INTRO (0:00 – 0:45)

> *[Screen: Your live blog homepage]*

**SAY:**
"Hey everyone. I built this entire blog — dark mode, comments, image lightbox, keyboard shortcuts — in a single session. No React. No frameworks. Just HTML, CSS, and Markdown."

"And I did it with AI as my co-pilot. I'm going to show you exactly how — and more importantly, how YOU can fork this and have your own blog running in under 2 minutes. Let's go."

---

## PART 1: THE IDEA (0:45 – 1:30)

> *[Screen: Show the kipp.ly reference site briefly, then your site]*

**SAY:**
"I wanted something like this — clean, typography-first, no distractions. But I also wanted features that actually matter: dark mode, RSS, comments, a reading progress bar."

"The entire tech stack is: Jekyll for static site generation, GitHub Pages for free hosting, and vanilla CSS. That's it. Zero JavaScript frameworks."

---

## PART 2: HOW TO FORK & USE IT (1:30 – 4:00)

> *[Screen: GitHub repo page]*

**SAY:**
"Step one — go to my GitHub repo. Link's in the description. Click **Fork**."

> *[Screen: Fork the repo, show Settings → Pages]*

"Step two — in your forked repo, go to **Settings → Pages**, make sure it's deploying from the `main` branch. That's it. Your site is live at `yourusername.github.io`."

> *[Screen: Show _config.yml]*

"Step three — open `_config.yml` and change the title, description, and URL to yours."

```yaml
title: Your Blog Name
description: Your description
url: "https://yourusername.github.io"
```

### Show the file structure

> *[Screen: VS Code or terminal showing the file tree]*

**SAY:**
"Here's how the project is organized:"

- `_posts/` — Your blog posts. One Markdown file per post.
- `_layouts/` — The HTML templates. You probably don't need to touch these.
- `assets/style.css` — All the styling. CSS variables for theming.
- `index.md` — Your homepage content. Edit the quote and tasks here.
- `philosophy.md`, `sops.md`, `now.md` — Other pages.

### Write a post

> *[Screen: Create a new file in _posts/]*

**SAY:**
"To write a post, create a file in `_posts/` with this naming format:"

```
2026-03-19-your-title.md
```

"Add frontmatter at the top:"

```yaml
---
layout: post
title: "Your Post Title"
description: "A one-liner for SEO"
tags: [tag1, tag2]
date: 2026-03-19 14:00:00 +0700
---

Your markdown content here.
```

"Push to GitHub, and it goes live in about 60 seconds."

---

## PART 3: RUNNING LOCALLY (4:00 – 5:30)

> *[Screen: Terminal]*

**SAY:**
"If you want to preview before pushing, here's how to run it locally."

```bash
# Install Jekyll (one-time setup)
gem install bundler jekyll

# Clone your repo
git clone https://github.com/YOUR_USERNAME/YOUR_USERNAME.github.io.git
cd YOUR_USERNAME.github.io

# Run the local server
jekyll serve
```

"Now open `http://localhost:4000` in your browser. Every time you save a file, Jekyll rebuilds automatically."

> *[Screen: Show localhost:4000 with the site running]*

"This is exactly what GitHub Pages does — but on your machine. Perfect for testing before you push."

---

## PART 4: FEATURES WALKTHROUGH (5:30 – 7:30)

> *[Screen: Your live site — click through each feature]*

**SAY:**
"Let me quickly walk through what you get out of the box:"

### Dark Mode
> *[Click the ◑ toggle]*
"Click this button. It remembers your preference."

### Image Lightbox
> *[Click an image in a post]*
"Click any image to zoom. You can drag to pan around. Press Escape to close."

### Keyboard Navigation
> *[On homepage, press j, j, k, Enter]*
"Press **j** to go down, **k** to go up, **Enter** to open. Vim-style."

### Comments
> *[Scroll to Discuss section]*
"Comments powered by giscus — uses GitHub Discussions. Zero backend, zero cost. Visitors sign in with GitHub."

### Reading Time & Progress
> *[Open a post, scroll slowly]*
"Reading time badges on the homepage. And this red progress bar at the top tracks your scroll."

### Table of Contents
> *[Show the sticky TOC on a post]*
"On wider screens, posts get an auto-generated table of contents that highlights as you scroll."

---

## PART 5: THE AI ANGLE (7:30 – 9:00)

> *[Screen: Split view — your editor on one side, the AI chat on the other]*

**SAY:**
"Now here's the part I want to be transparent about. I built this with AI assistance. Specifically:"

- "**Google Gemini** — as my primary pair programmer"
- "**GitHub Copilot** — for inline suggestions"
- "**ChatGPT** — for brainstorming and research"

"The AI wrote code. I made decisions. I asked questions, I understood the answers, and I directed every feature."

"This isn't about replacing developers. It's about **multiplying productivity**. What would have taken a weekend of Stack Overflow and documentation took one focused session."

> *[Screen: The blog post "How I Built This Blog With AI"]*

"I wrote a full blog post about this — linked in the description. It covers the ethical argument for crediting AI, what I actually learned, and the exact tools I used."

---

## OUTRO (9:00 – 9:30)

> *[Screen: GitHub repo]*

**SAY:**
"The entire thing is open source under MIT license. Fork it, customize it, make it yours."

"Links in the description:"
- "GitHub repo"
- "Live site"
- "The blog post about building with AI"

"If you found this useful, like, subscribe, and drop a comment — ironically, powered by GitHub Discussions."

"See you in the next one."

---

## VIDEO DESCRIPTION (Copy-paste)

```
🔗 Links:
Live Site: https://sai21112000.github.io
GitHub Repo: https://github.com/Sai21112000/sai21112000.github.io
Blog Post: https://sai21112000.github.io/posts/building-with-ai.html

📦 Tech Stack:
- Jekyll (Static Site Generator)
- GitHub Pages (Free Hosting)
- Vanilla CSS (No Frameworks)
- Giscus (GitHub Discussions Comments)
- Zero JavaScript Frameworks

🤖 AI Tools Used:
- Google Gemini (Antigravity Agent)
- GitHub Copilot
- ChatGPT
- Obsidian (Writing)

⏱ Timestamps:
0:00 Intro
0:45 The Idea
1:30 Fork & Setup (2-minute deploy)
4:00 Running Locally
5:30 Features Walkthrough
7:30 Building With AI — The Honest Take
9:00 Outro

#OpenSource #WebDev #AIAssistedDevelopment #Jekyll #GitHubPages #BuildInPublic
```

---

## THUMBNAIL IDEAS

1. Split screen: your blog on left, terminal on right, text overlay "Built in 1 Session"
2. Your blog homepage with AI-themed glow effects, text "AI + HTML = This Blog"
3. Clean shot of the dark mode site with "Zero Frameworks. Free Hosting. Open Source."
