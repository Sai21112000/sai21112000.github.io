---
layout: post
title: "Getting Started with This Blog"
date: 2026-03-03
categories: [meta, research]
tags: [jekyll, github-pages, markdown]
---

Welcome to my blog! This site is built with **Jekyll** and hosted on **GitHub Pages** — no server setup or extra configuration required.

## Why Jekyll on GitHub Pages?

- **Zero configuration** — push Markdown files and GitHub builds the site automatically
- **Markdown-based** — write posts just like you write README files
- **Code highlighting out of the box** — perfect for sharing technical content
- **Academic-friendly** — ideal for documenting research and thesis work

## Writing Posts

Every post is a Markdown file in the `_posts/` folder, named with the format `YYYY-MM-DD-title.md`. The front matter at the top sets the title, date, and categories.

## Code Syntax Highlighting

Jekyll uses **Rouge** for syntax highlighting. Here's an example in Python:

```python
def fibonacci(n):
    """Return the nth Fibonacci number."""
    if n <= 1:
        return n
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b

print(fibonacci(10))  # Output: 55
```

And here's an example in Bash:

```bash
# Build and serve the site locally
bundle exec jekyll serve --livereload
```

## What's Next?

I'll be using this space to share:
1. **Thesis progress** — findings and methodology notes
2. **Technical deep-dives** — walkthroughs with code examples
3. **Research summaries** — distilling papers into digestible posts

Stay tuned!
