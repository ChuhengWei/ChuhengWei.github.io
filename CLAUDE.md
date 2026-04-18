# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A personal academic website for Chuheng Wei (PhD student at UC Riverside), built on the [academicpages](https://academicpages.github.io/) Jekyll theme — itself a fork of Minimal Mistakes. It is hosted via GitHub Pages at https://ChuhengWei.github.io. Pushing to `main` triggers automatic deployment.

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

## Homepage Layout (`_pages/about.md`)

The homepage does **not** use the sidebar author profile (`author_profile: false`). Instead it has a fully custom layout:

- **`.profile-hero`** flex container: bio text on the left, photo + social icons on the right
- **`.news-scroll`** — a `max-height: 480px; overflow-y: auto` scrollable div. News entries are markdown list items with `[MM/YYYY]` date prefixes inside `<div class="news-scroll" markdown="1">`.
- All CSS lives in a `<style>` block at the bottom of `about.md` — there is no separate stylesheet for the homepage layout. Edit styles there.
- Profile photo is at `/images/kxm.jpg`.

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
