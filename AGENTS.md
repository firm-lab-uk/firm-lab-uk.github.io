# AGENTS.md

Hugo static site for the FIRM lab (PaperMod theme), deployed to GitHub Pages.

## Commands
- Local dev: `hugo server` (http://localhost:1313, live-reloads)
- Build: `hugo` (writes to `public/`)
- No npm/Node, tests, or lint tooling. `.markdownlint.json` disables MD013/024/025/045/049.
- Deploy: push to `main` → `.github/workflows/hugo.yml` builds with Hugo v0.147.2 (extended) and publishes to GitHub Pages automatically.

## Structure & gotchas
- `themes/PaperMod/` is a vendored copy (not a submodule) of the theme.
- `layouts/` and `assets/css/` at the repo root are full copies of the theme's templates/CSS, heavily pruned and customized. Root copies shadow the theme files — edit the root copies, never `themes/PaperMod/`. Site-specific CSS goes in `assets/css/extended/custom.css`.
- Config is `config.yaml` (README's `config.yml` references are stale). No root `go.mod`; the theme loads from `themes/` (no Hugo modules).
- Local Hugo is v0.164.0+extended vs CI's pinned v0.147.2 — rebuild with `--minify` before assuming parity.
- Build artifacts are gitignored: `public/`, `resources/`, `.hugo_build.lock`.
- Math: KaTeX via CDN, on globally (`params.math: true`). Use `$...$` inline / `$$...$$` display. `markup.goldmark.renderer.unsafe: true` — raw HTML is allowed in Markdown.
- Custom link renderer (`layouts/_default/_markup/render-link.html`) auto-opens external `http` links in a new tab.

## Content conventions
- People: `content/people/<name>/index.md` + square `profile.png` (thumbnailed 300×300). Front matter uses `weight` (sort order), `summary`, `hideMeta: true`, and `social:` whose `name` must match an icon in `layouts/partials/svg.html` (email, google scholar, orcid, webpage, researchgate, …).
- Publications: `content/publications/<slug>/index.md` + relative cover image; `links:` entries use `url` or `file` (e.g. pdf). `venue` renders next to the date on the card. Abstracts use KaTeX.
- Publications list grouping is driven by `layouts/publications/list.html` (+ `layouts/partials/publication-card.html`): cards split into **Published** and **Pending** headings by the front-matter `status` field, then grouped by year (newest first). Valid `status` values: `published`, `in prep`, `in review`, `submitted`, `in production`. Folders follow `YYYY_firstauthor_published` / `YYYY_firstauthor_pending` naming (suffix is cosmetic; `status` is authoritative). Year headings come from the `date:` field.
- News: `content/news/YYYY-MM-DD-slug.md`. Research: `content/research/wpN/index.md`, ordered by `weight`.
- Section landing pages are `_index.md`; the tags taxonomy is defined in `config.yaml`.
- Some content is still placeholder (e.g. `[Lead name]`, lorem ipsum) — don't assume it's final.
