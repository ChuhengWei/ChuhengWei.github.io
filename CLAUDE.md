# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A personal academic website for Chuheng Wei (Applied Scientist at Amazon Prime Video; PhD from UC Riverside), built on the [academicpages](https://academicpages.github.io/) Jekyll theme — itself a fork of Minimal Mistakes. It is hosted via GitHub Pages at https://chuhengwei.com (custom domain; ChuhengWei.github.io redirects there). Pushing to `main` triggers automatic deployment.

## Local Development

```bash
bundle install              # install Ruby dependencies (run once)
bundle exec jekyll liveserve  # serve at localhost:4000 with live reload (requires hawkins gem)
```

For local dev, Jekyll uses `_config.yml` merged with `_config.dev.yml` (overrides URL to `localhost:4000` and disables analytics/comments). To use the dev config explicitly:

```bash
bundle exec jekyll serve --config _config.yml,_config.dev.yml
```

If you get bundle errors, delete `Gemfile.lock` and re-run `bundle install`.

## Content Architecture

Content lives in Jekyll **collections** — each folder maps to a URL path:

| Folder | URL | Purpose |
|---|---|---|
| `_pages/about.md` | `/` (homepage) | Main bio, News section, Professional Service |
| `_publications/` | `/publications/` | One `.md` per paper |
| `_talks/` | `/talks/` | Conference talks |
| `_teaching/` | `/teaching/` | Teaching entries |
| `_posts/` | `/year-archive/` | Blog posts |
| `_portfolio/` | `/portfolio/` | Portfolio items |
| `files/` | `/files/` | PDFs and downloads |

**Navigation** is controlled by `_data/navigation.yml`. All nav links are currently commented out — uncomment to add them to the site header.

## Key Configuration

- `_config.yml` — site-wide settings: author info, social links, analytics, collection permalinks
- `_data/authors.yml` — author metadata used in templates
- `_data/ui-text.yml` — UI string overrides for internationalization

## Split-Screen Layout (homepage + Publications)

`_pages/about.md` and `_pages/publications.md` both use `layout: split` (`_layouts/split.html`), a custom layout that hides the theme masthead/footer and renders:

- **Left panel** (`_includes/split-panel.html`): sticky dark column with the cover photo (`/images/kxm.jpg`), name, title, social icons and a numbered section nav. Nav anchors point to homepage sections; "Publications" links to `/publications/`. The active item is set from `page.url`. Edit name/title/links there.
- **Right column** (`<main class="content">`): the page body, written as raw HTML `<section id="...">` blocks with an `<h2>`.
- Homepage sections in order: about, experience, education, news, publications (Selected Publications: four `.selpub` cards linking to `/publications/`), service, visitors. The nav numbering in `split-panel.html` must follow this order.
- **All styling** is in `assets/css/split.css` (Manrope font loaded from Google Fonts). The root font size is pinned to 15px there because the theme bumps `html` to 18–20px on desktop.

Homepage content conventions (`about.md`):

- Experience/Education entries are `.row` blocks with a `.tile` logo (`/images/logos/*.svg|png`), org/role/location text and a `.date`. Add `<span class="tag">Current</span>` after the org for the current position.
- News is an HTML grid: pairs of `<div class="d">MM/YYYY</div><div class="t">text</div>`. The first 12 pairs are visible; the rest sit inside `<div class="hidden-items">` and are revealed by the "Show all N updates" button (update N in both `data-more-label` and the button text when adding items).
- Reviewer lists are `.chips` (`<a>` or `<span>` per venue).

`_pages/publications.md` keeps its hand-written `.pub-card` list and Cite modal; its styles also live in `split.css`. The CV page (`_pages/cv.md`) still exists but is no longer linked from navigation.

## Publication Front Matter

Each file in `_publications/` uses this structure:

```yaml
---
title: "Paper Title"
collection: publications
permalink: /publication/YYYY-slug
date: YYYY-MM-DD
venue: 'Journal or Conference Name'
citation: 'Full citation string with HTML entities for quotes'
---
[Download paper here](http://ChuhengWei.github.io/files/paperN.pdf)
```

Note: `_pages/cv.md` still contains template placeholder content (GitHub University) — it has not been updated with real CV data.

## Markdown Generator

The `markdown_generator/` folder contains Python/Jupyter scripts to batch-generate publication and talk `.md` files from TSV data files — useful for bulk imports.
