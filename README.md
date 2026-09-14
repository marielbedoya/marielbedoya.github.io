# marielbedoya.com

Source for my academic website. Static site built with [Hugo](https://gohugo.io/)
and a customised [academimal](https://github.com/yangl1996/academimal) theme,
deployed to GitHub Pages.

## Structure

Single-page layout. Sections render in this order and each is omitted
automatically when its source is absent:

`Bio → Job Market Paper → Publications → Working Papers → Work in Progress → Policy Work → Teaching → Contact`

| Path | Contents |
|---|---|
| `config.toml` | Site title, short bio, sidebar links (CV, email, Scholar, LinkedIn) |
| `content/sections/*.md` | Prose sections — bio, teaching, contact |
| `data/<section>/*.yaml` | Papers and reports, one directory per section |
| `layouts/` | Project-level template overrides |
| `static/pdf/` | Served at `/pdf/` — CV, drafts, appendices |
| `static/css/custom.css` | Palette and styling on top of the theme |

Multiple `.yaml` files in one `data/` directory are concatenated, so long lists
can be split by year or project.

## Entry schema

Only `title` is required.

```yaml
works:
- title: "Title of the paper"
  pdflink: "/pdf/paper.pdf"                 # makes the title a link
  coauthors: "Coauthor A and Coauthor B"    # renders as "(with ...)"
  book: "Journal Name, 2026"                # italicised outlet line
  note: "Revise and resubmit"               # plain line beneath coauthors
  links:
    - url: "/pdf/appendix.pdf"
      text: "Online Appendix"
  abstract: >
    Rendered behind a click-to-expand toggle.
```

## Local development

```bash
hugo server -D          # http://localhost:1313, live reload
```

Requires Hugo (`brew install hugo`). If `themes/academimal` is empty after a
fresh clone, the submodule was not fetched:

```bash
git submodule update --init --recursive
```

## Deployment

`.github/workflows/hugo.yaml` builds and publishes to GitHub Pages on every
push to `main`. Custom domain is set in `static/CNAME`.

## Implementation notes

**The theme submodule is pinned to `acf9ebb`.** Upstream `master` has since been
restructured and no longer uses the data-file schema above. Do not run
`git submodule update --remote`.

**Templates are overridden at project level** rather than by editing the
submodule, so the theme stays updatable in principle:

- `layouts/index.html` — section order, plus the Job Market Paper, Policy Work
  and Teaching sections, which the theme does not provide
- `layouts/partials/sidebar.html` — navigation, kept in sync with the above
- `layouts/partials/header.html` — sidebar link block, favicon, meta tags
- `layouts/partials/foot.html` — replaces a Google Analytics template removed
  from Hugo after v0.120, so the site builds on current versions

**Palette.** Accent `#0f7b5f`, hover `#0a5a45`, solid ground `#0c5f49`. Chosen
for contrast against the background *and* separation from body text; the
reasoning and the measured alternatives are documented in `static/css/custom.css`.
