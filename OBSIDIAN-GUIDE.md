# Obsidian → GitHub Pages: Cheat Sheet

A quick reference for copying notes from Obsidian to your Jekyll blog.

---

## ⚡ Pre-Flight Checklist

Before you push, make sure your `.md` file has:

```
✅ YAML frontmatter at the very top (between --- lines)
✅ Standard Markdown image syntax (not Obsidian ![[]] syntax)
✅ No Obsidian-only callouts (convert > [!NOTE] to > **Note:**)
✅ File is inside _posts/ with correct date-naming
✅ Date in filename is today or earlier (NOT future)
```

---

## 📄 Post Template

Copy this into every new post:

```markdown
---
layout: post
title: "Your Post Title Here"
description: "A one-line summary for SEO and social sharing."
tags: [tag1, tag2, tag3]
last_modified: 2026-03-19
---

Your content starts here. Write normally in Markdown.

## Section Heading

Regular paragraph text.

> Blockquotes look like this.

**Bold text**, *italic text*, `inline code`.
```

### File Naming Rule
```
_posts/YYYY-MM-DD-your-post-slug.md
```
Example: `_posts/2026-03-19-how-vpns-work.md`

> ⚠️ **The date MUST be today or earlier.** Jekyll ignores future-dated posts!

---

## 🖼️ Images

### Obsidian Syntax (WON'T work)
```markdown
![[my-screenshot.png]]
![[folder/image.png|500]]
```

### Jekyll Syntax (USE THIS)
```markdown
![Description of image](/assets/images/my-screenshot.png)
```

### Steps:
1. Move your image file into `assets/images/`
2. Use the path `/assets/images/filename.png` (NOT the full Mac path)
3. Never use `/Users/apple/...` — GitHub doesn't know your computer

---

## 📝 Callouts / Admonitions

### Obsidian (WON'T work)
```markdown
> [!NOTE] This is a note
> Content here

> [!WARNING] Be careful
> Content here
```

### Jekyll (USE THIS)
```markdown
> **Note:** This is a note.
> Content here.

> **Warning:** Be careful.
> Content here.
```

---

## 📎 Footnotes

Kramdown (Jekyll's Markdown engine) supports footnotes natively:

```markdown
This claim needs a source[^1].

Another point worth citing[^2].

[^1]: Author Name, "Article Title", 2026.
[^2]: Source URL or book reference.
```

This renders as superscript numbers that link to footnotes at the bottom of the page.

---

## 🔗 Links

### Obsidian Internal Links (WON'T work)
```markdown
[[Other Note]]
[[Other Note|Display Text]]
```

### Standard Markdown (USE THIS)
```markdown
[Display Text](https://example.com)
[Another Post](/posts/2026-03-19-other-post/)
```

---

## 📋 Philosophy Page Template

Add quotes to `philosophy.md`:

```markdown
> "Your quote text here."
{: .animate-enter style="animation-delay: 0.2s;"}

Your notes about this quote go here.
{: .quote-note .animate-enter style="animation-delay: 0.3s;"}
```

---

## 📋 SOPs Page Template

Add proposals to `sops.md`:

```markdown
- **Project Name** — Short description. [Download PDF](/assets/pdfs/file.pdf)
  *Status: In Progress*
```

---

## 📋 Homepage Quick Notes

Edit `index.md` YAML frontmatter:

```yaml
featured_quote: "Your featured quote"
featured_quote_source: "— Source"
tasks:
  - text: "Task description"
    done: false
  - text: "Completed task"
    done: true
```

---

## 🚀 Push Workflow

```bash
git add .
git commit -m "describe your change"
git push origin main
```

Site rebuilds automatically in ~60 seconds.

---

## ❌ Common Mistakes

| Mistake | Fix |
|---------|-----|
| `![[image.png]]` | `![alt](/assets/images/image.png)` |
| `> [!NOTE]` | `> **Note:**` |
| `[[Other Note]]` | `[Text](url)` |
| Future date in filename | Use today's date or earlier |
| No `---` frontmatter | Add YAML block at top of file |
| Full Mac path in image | Use `/assets/images/` only |
