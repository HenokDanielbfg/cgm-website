# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static website for **Christ Generation Ministry (CGM)** — a church based in the UAE. It has no build step, no framework, and no package manager. All files are plain HTML, CSS (inline), JavaScript (inline), and JSON data files.

To preview the site, open `index.html` in a browser or serve the folder with any static file server:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

A local server is required (not just opening the HTML file directly) because the page uses `fetch()` to load JSON/Markdown files, which fails under `file://` due to CORS restrictions.

## Architecture

All CSS and JavaScript are **inline** within each HTML file — there are no external `.css` or `.js` files.

### Pages
- `index.html` — Main landing page with all sections: Hero, Mission, Teaching, Articles, Services, Devotionals, Testimonies, Contact
- `article.html` — Individual article reader; reads `?id=` query param, looks up the article in `articles.json`, then fetches and renders the Markdown file

### Data Files (JSON)
- `teachings.json` — Array of teaching video objects: `{ title, excerpt, thumbnail, video }`; rendered as cards in the Teaching section
- `articles.json` — Array of article metadata objects: `{ id, title, excerpt, date, tag, md }`; the `md` field is a relative path to the Markdown source file
- `devotional.json` — Single object: `{ date, verse, reflection, prayer }`; rendered in the Devotionals section

### Article Content
- `articles/` — Markdown files for each article; referenced by the `md` field in `articles.json`
- `article.html` contains a custom minimal Markdown-to-HTML renderer (no external libraries); it supports headings (`##`, `###`), bold, italic, blockquotes, unordered lists, fenced code blocks, horizontal rules, and inline links

### Design System
The site uses a consistent dark theme with CSS custom properties defined in each file's `:root` block:
- `--bg`, `--panel`, `--text`, `--muted` — layout/text colors
- `--gold`, `--gold2`, `--gold3` — the gold accent palette
- `--border`, `--shadow`, `--radius` — card/UI utilities

Reusable classes: `.card`, `.pill`, `.btn` (with modifiers `.btn-primary`, `.btn-ghost`, `.btn-outline`), `.grid-2`, `.grid-3`, `.wrap`, `.serif`

## Content Updates

**Adding a teaching video:** Add an entry to `teachings.json` with `title`, `excerpt`, `thumbnail` (YouTube `hqdefault.jpg` URL), and `video` (YouTube URL).

**Adding an article:**
1. Create a Markdown file in `articles/`
2. Add a metadata entry to `articles.json` with a unique `id`, `title`, `excerpt`, `date` (ISO format), `tag`, and `md` path

**Updating the devotional:** Edit `devotional.json` — only one devotional is shown at a time.
