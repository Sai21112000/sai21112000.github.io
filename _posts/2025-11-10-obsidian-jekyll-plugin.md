---
layout: post
title: "I Built an Obsidian Plugin to Publish to Jekyll"
description: "An open-source Obsidian plugin that converts wiki-links, image embeds, and callouts to Jekyll-compatible Markdown — then copies everything to your _posts/ folder."
tags: [obsidian, jekyll, open-source, productivity]
date: 2025-11-10 00:30:00 +0700
---

I write everything in Obsidian. My blog runs on Jekyll. The two speak different dialects of Markdown.

So I built a plugin that translates between them.

## The Problem

Obsidian uses `![[image.png]]` for images. Jekyll wants `![image](/assets/images/image.png)`.

Obsidian uses `[[Page Link]]` for internal links. Jekyll wants `[Page Link](/posts/page-link.html)`.

Obsidian has callouts like `> [!NOTE]`. Jekyll has no idea what those are.

Every time I wanted to publish, I'd spend 10 minutes manually fixing syntax. That's 10 minutes of pure friction between writing and shipping.

## The Solution

**Jekyll Publisher** — an Obsidian plugin that:

1. Converts all Obsidian-specific syntax to standard Markdown
2. Injects missing Jekyll frontmatter (`layout`, `title`, `date`)
3. Generates the correct `YYYY-MM-DD-slug.md` filename
4. Copies the post to your `_posts/` directory
5. Copies referenced images to `assets/images/`

One command. Zero manual editing.

## How to Use It

Open a note. Press `Cmd+P`. Type "Publish to Jekyll". Done.

Or click the 📤 icon in the sidebar for even less friction.

The plugin handles everything:

| What It Converts | From | To |
|-----------------|------|-----|
| Images | `![[photo.png]]` | `![photo](/assets/images/photo.png)` |
| Links | `[[Other Note]]` | `[Other Note](/posts/other-note.html)` |
| Callouts | `> [!WARNING]` | `> **Warning:**` |
| Highlights | `==text==` | `<mark>text</mark>` |
| Comments | `%%hidden%%` | *(removed)* |

## Open Source

The entire plugin is MIT-licensed and on GitHub: [obsidian-jekyll-publisher](https://github.com/Sai21112000/obsidian-jekyll-publisher)

It's ~8KB minified. No external dependencies beyond Obsidian's API.

Fork it, customize it, improve it. PRs welcome.

---

*This post was published using the plugin itself.*
