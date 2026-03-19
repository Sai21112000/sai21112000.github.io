# Sai's Blog

A minimalist, Markdown-driven, zero-JS blog built natively with GitHub Pages (Jekyll) and customized with deep CSS aesthetics. 

## Features
- **Obsidian-Ready**: Write entirely in Markdown (`.md`). GitHub handles the heavy lifting of turning it into a beautiful webpage.
- **Typography-first**: Crimson Pro and IBM Plex Mono.
- **Ultra-fast**: No JavaScript, raw CSS animations.

## How to use your blog

### 1. Adding a new Blog Post
- Open the `_posts/` folder.
- Create a new file with the date format: `2026-03-20-post-name.md`.
- Add YAML frontmatter at the top:
  ```yaml
  ---
  layout: post
  title: "Your Title"
  tags: [obsidian, design]
  ---
  ```
- Write your content below using standard Markdown!

### 2. Updating your "Selected Work" Projects
- Open `index.md`.
- Edit the `projects:` list inside the top YAML block. You can safely add new portfolio items here without ever touching HTML.

### 3. Adding quotes to "Philosophy"
- Open `philosophy.md`.
- Use standard Markdown `>` for quotes.
- The special `{: .animate-enter...}` lines immediately below text blocks instruct the Jekyll system to apply our beautiful aesthetic layout CSS!

### Deployment
Any time you `git push` to `main`, GitHub Pages automatically compiles your Markdown and updates your live site!
