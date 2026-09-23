# exarep.github.io

Documentation site for the Exarep organization — a reference architecture for running microservices on Red Hat OpenShift, modeled as a retail electricity provider in ERCOT, Texas.

Built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material).

## Getting Started

Clone the repository:

```bash
git clone https://github.com/exarep/exarep.github.io.git
cd exarep.github.io
```

Create and activate the Python virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Start the development server:

```bash
mkdocs serve --livereload
```

The site will be available at [http://localhost:8000](http://localhost:8000).

## Deployment

The site is automatically deployed to GitHub Pages via GitHub Actions on push to the `main` branch.
