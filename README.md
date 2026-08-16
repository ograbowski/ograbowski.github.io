# Personal site

Personal website + blog, hosted on GitHub Pages (Jekyll, built automatically by GitHub).

## How to add a new post

Create a file in `_posts/` named `YYYY-MM-DD-post-title.md`, e.g. `_posts/2026-08-16-first-post.md`:

```markdown
---
layout: post
title: "Post title"
---

Post content in Markdown...
```

After `git push`, GitHub Pages rebuilds the site automatically (usually within ~1 minute).
The post will appear in the list on the home page at `/blog/YYYY/post-title/`.

## Structure

- `index.md` — home page (landing page + post list)
- `_posts/` — blog posts
- `_layouts/` — HTML templates (default, post)
- `assets/css/style.css` — styles
- `_config.yml` — Jekyll configuration
