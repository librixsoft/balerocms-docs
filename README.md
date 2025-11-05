# BaleroCMS Documentation

This repository contains the MkDocs site for BaleroCMS docs.

## Requirements

- Python 3.8+
- pip

## Setup

### 1. Create a virtual environment (recommended)

```bash
python3 -m venv .venv
```

### 2. Activate the virtual environment

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install mkdocs mkdocs-material mike
```

## Development

### Run locally (preview without versions)

```bash
mkdocs serve
```

Then open: http://127.0.0.1:8000

### Preview with versions (using Mike)

```bash
mike serve
```

Then open: http://127.0.0.1:8000

## Build

### Build the static site

```bash
mkdocs build --clean
```

This generates the static output under `site/`.

## Deployment

### Using Mike (recommended for versioned docs)

Mike handles both building and deploying to GitHub Pages:

```bash
# Example: Deploy version 1.0 with "latest" alias
mike deploy 1.0 latest --update-aliases --allow-empty
git push origin gh-pages
```

### Create and deploy a new version

When you're ready to publish a new version (e.g., 1.1):

```bash
# Deploy version 1.1 and move the "latest" alias to it
mike deploy 1.1 latest --update-aliases --allow-empty

# Push to GitHub Pages
git push origin gh-pages

# Optional: Set as default version (landing page)
mike set-default 1.1
git push origin gh-pages
```

After deployment, users will see a version selector in the documentation with both 1.0 and 1.1 available, and 1.1 will be marked as "latest".

### Manual deployment (without versions)

If not using Mike:

```bash
mkdocs gh-deploy --clean --force
```

- `--clean`: remove obsolete files from gh-pages
- `--force`: force push even if the branch exists locally

**Note:** When using Mike, don't use `mkdocs gh-deploy` as Mike manages the `gh-pages` branch.