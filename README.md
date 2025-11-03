# BaleroCMS Documentation

This repository contains the MkDocs site for BaleroCMS docs.

## Requirements

- Python 3.8+
- pip

## Install dependencies

pip install mkdocs mkdocs-material

## Run locally (preview)

mkdocs serve

Then open:

http://127.0.0.1:8000

## Build the static site

mkdocs build --clean

This generates the static output under `site/`.

## Deploy to GitHub Pages (manual)

The site is configured for GitHub Pages (see `site_url` in `mkdocs.yml`).

mkdocs gh-deploy --clean --force

- --clean: remove obsolete files from gh-pages.
- --force: force push even if the branch exists locally.

Make sure you have push permissions and that Pages is set to serve from the gh-pages branch in the repository settings.

## Project structure

- docs/ — Markdown content
- mkdocs.yml — MkDocs configuration (theme, navigation, extensions)
- site/ — Build output (ignored by git)

## Versioning with Mike

Enable versioned docs using Mike (works with Material for MkDocs).

### 1) Install Mike

pip install mike

# If using pipx:
# pipx install mike

### 2) Configure Material version selector

In mkdocs.yml:

extra:
  version:
    provider: mike

### 3) Prepare GitHub Pages

- Ensure GitHub Pages serves from the gh-pages branch.
- The first Mike deployment will create/update this branch.

### 4) Create the first version and set default

# Deploy version 1.0 and label it as "latest"
mike deploy 1.0 latest -u

# Make "latest" the default version on the site
mike set-default latest -u

### 5) Add a new version later

After updating docs:

# Publish as 1.1 and move the "latest" alias
mike deploy 1.1 latest -u

### 6) Preview locally with versions

mike serve

### 7) Useful commands

# List published versions
mike list

# Retitle a version
mike retitle 1.0 "1.0 (Initial Release)"

# Delete a version and push the change
mike delete 1.0 -p

Notes:
- mike manages content on the gh-pages branch; typically you don’t use mkdocs gh-deploy when using Mike.
- Keep site/ ignored; Mike builds and pushes the output for you.

## Recommended workflow (stable, avoids pipx issues)

To update version 1.0 reliably:

1. Create a **virtual environment** (only once):

python3 -m venv .venv

2. Activate it whenever you open a new terminal:

source .venv/bin/activate

3. Install all required packages (only once):

pip install mkdocs mkdocs-material mike

4. Update your docs in `docs/` as needed.

5. Deploy the updated version 1.0:

mike deploy 1.0 --push

6. Optional: verify in browser:

http://127.0.0.1:8000/1.0/

With this workflow, you don’t need to reinstall anything, and MkDocs will always recognize the Material theme.
