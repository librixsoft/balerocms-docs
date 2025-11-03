# BaleroCMS Documentation

This repository contains the MkDocs site for BaleroCMS docs.

## Requirements

- Python 3.8+
- pip

## Install dependencies

```bash
pip install mkdocs mkdocs-material
```

## Run locally (preview)

```bash
mkdocs serve
```

Then open:

```
http://127.0.0.1:8000
```

## Build the static site

```bash
mkdocs build --clean
```

This generates the static output under `site/`.

## Deploy to GitHub Pages

The site is configured for GitHub Pages (see `site_url` in `mkdocs.yml`).

Manual deployment (creates/updates the `gh-pages` branch):

```bash
mkdocs gh-deploy --clean --force
```

- `--clean`: remove obsolete files from `gh-pages`.
- `--force`: force push even if the branch exists locally.

Make sure you have push permissions and that Pages is set to serve from the `gh-pages` branch in the repository settings.

## Project structure

- `docs/` — Markdown content
- `mkdocs.yml` — MkDocs configuration (theme, navigation, extensions)
- `site/` — Build output (ignored by git)


