# exarep.github.io

Documentation site for Exarep, built with [MkDocs](https://www.mkdocs.org/) and the [Material](https://squidfunk.github.io/mkdocs-material/) theme.

## Local Development

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve --livereload
```

The site will be available at `http://127.0.0.1:8000`.

## Build

```bash
mkdocs build
```

## Deployment

The site deploys automatically to GitHub Pages on push to `main` via GitHub Actions.
