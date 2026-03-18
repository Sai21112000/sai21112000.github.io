# sai's blog

A minimalist, zero-JS blog built with pure HTML5 and CSS3. Inspired by kipp.ly and the `frontend-specialist` deep design guidelines.

## Features
- **Typography-first**: Crimson Pro and IBM Plex Mono.
- **Ultra-fast**: No JavaScript, no bundlers, raw CSS.
- **Dark Mode**: Native `prefers-color-scheme` support.
- **Radical Geometries**: Sharp edges, heavy contrast link interactions, asymmetric typographic emphasis. (No Bento, No Purple).

## How to add a post
1. Copy `posts/first-post.html` to a new file like `posts/my-new-post.html`.
2. Edit the title, `<meta name="description">`, date, and tags.
3. Write your content using standard HTML (`<h2>`, `<p>`, `<blockquote>`, `<pre><code>`).
4. Update `index.html` to include a link to your new post in the `chronological` section, and optionally in the `favourites` section.
5. Push your changes!

## Deployment
Since this repo is `sai21112000.github.io`, it will deploy automatically via **GitHub Pages**. To enable:
1. Go to your repository **Settings** on GitHub.
2. Select **Pages** from the sidebar.
3. Under Build and deployment, set source to **Deploy from a branch**.
4. Select the **main** branch and **/(root)** folder.
5. Click **Save**. Your site will be live in a few minutes!
