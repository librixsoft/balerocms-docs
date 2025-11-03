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

## Versioning with mike

Enable versioned docs with the `mike` plugin (works with Material for MkDocs).

1) Install `mike`:

```bash
pip install mike
```

2) Configure Material’s version selector in `mkdocs.yml`:

```yaml
extra:
  version:
    provider: mike
```

3) Prepare GitHub Pages

- Ensure GitHub Pages serves from the `gh-pages` branch.
- The first `mike` deployment will create/update `gh-pages`.

4) Create the first version and set default

```bash
# Deploy version 1.0 and label it as "latest"; -u updates the alias
mike deploy 1.0 latest -u

# Make "latest" the default version on the site
mike set-default latest -u
```

5) Add a new version later

```bash
# After updating docs, publish as 1.1 and move the "latest" alias
mike deploy 1.1 latest -u
```

6) Preview locally with versions

```bash
mike serve
```

7) Useful commands

```bash
# List published versions
mike list

# Retitle a version
mike retitle 1.0 "1.0 (Initial Release)"

# Delete a version and push the change
mike delete 1.0 -p
```

Notes:
- `mike` manages content on the `gh-pages` branch; you typically don’t use `mkdocs gh-deploy` with `mike`.
- Keep `site/` ignored; `mike` builds and pushes the output for you.


