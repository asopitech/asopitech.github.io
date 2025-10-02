# asopitech.github.io

Asopitech's official website built with MkDocs and deployed on GitHub Pages.

## Features

- 🌐 Bilingual support (English and Japanese)
- 🎨 Modern Material Design theme
- 🌙 Dark/Light mode toggle
- 📱 Fully responsive
- 🔍 Built-in search functionality
- 🚀 Automatic deployment via GitHub Actions

## Local Development

### Prerequisites

- Python 3.x
- pip

### Installation

1. Clone the repository:
```bash
git clone https://github.com/asopitech/asopitech.github.io.git
cd asopitech.github.io
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

### Running Locally

Start the development server:
```bash
mkdocs serve
```

The site will be available at `http://localhost:8000`

- English version: `http://localhost:8000`
- Japanese version: `http://localhost:8000/ja/`

### Building

To build the static site:
```bash
mkdocs build
```

The generated site will be in the `site/` directory.

## Project Structure

```
.
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Actions deployment workflow
├── docs/
│   ├── index.md           # English landing page
│   └── index.ja.md        # Japanese landing page
├── mkdocs.yml             # MkDocs configuration
├── requirements.txt       # Python dependencies
└── README.md
```

## Deployment

The site is automatically deployed to GitHub Pages when changes are pushed to the `main` branch. The deployment is handled by the GitHub Actions workflow defined in `.github/workflows/deploy.yml`.

## Adding Content

### Adding a New Page

1. Create a new markdown file in the `docs/` directory (e.g., `docs/about.md`)
2. For Japanese version, create `docs/about.ja.md`
3. Update the `nav` section in `mkdocs.yml`:

```yaml
nav:
  - Home: index.md
  - About: about.md
```

### Modifying Existing Content

Simply edit the corresponding markdown files in the `docs/` directory:
- `docs/index.md` - English homepage
- `docs/index.ja.md` - Japanese homepage

## License

Copyright © 2024 Asopitech