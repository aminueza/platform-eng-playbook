# Platform Engineering: 90-Day Plan

Senior Platform Engineer (IC-3) assessment and roadmap for Moxie.

**Author:** Amanda Souza
**Date:** February 2026

---

## View the Plan

**[Read the full plan here](docs/index.md)**

Or build and serve locally:

```bash
pip install mkdocs-material
mkdocs serve
```

Then open http://localhost:8000

---

## Publishing to GitHub Pages

This site automatically deploys to GitHub Pages when pushed to the `main` branch.

To set up:

1. Go to your repository Settings → Pages
2. Set Source to "GitHub Actions"
3. Push the code to `main`

The site will be available at: `https://[username].github.io/[repo-name]/`

---

## Structure

- **Assessment** — Root cause analysis and priority ranking
- **Proposals** — Two concrete technical proposals with metrics
- **Roadmap** — Week-by-week execution plan with ownership
- **Communication** — Stakeholder communication strategy
- **Appendix** — Clarifying questions, microservices conversation, de-risking the 6-month feature

---

## Local Development

```bash
# Install dependencies
pip install -r requirements.txt

# Serve locally with live reload
mkdocs serve

# Build static site
mkdocs build
```
