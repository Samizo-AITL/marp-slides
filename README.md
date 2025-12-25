# marp-slides

This repository is a **Markdown-first presentation workspace** using **Marp**.

All slides are authored in Markdown and exported into multiple formats depending on purpose:

- **HTML** for GitHub Pages (publishing / sharing)
- **PowerPoint (.pptx)** for talks and offline presentations

The goal of this repository is to **separate thinking from formatting** and make slide creation reproducible, fast, and version-controlled.

---

## Philosophy

- **Markdown is the single source of truth**
- **HTML is the published artifact**
- **PowerPoint is the presentation artifact**
- GitHub Pages does **not** execute Marp

This clear separation prevents common issues such as:
- "Markdown updated but the page did not change"
- "PowerPoint is out of sync with the source"

---

## Repository Structure

```text
marp-slides/
├ slides/        # Marp source files (Markdown)
│  ├ physical-first.md
│  └ aitl-architecture.md
│
├ dist/          # Generated artifacts
│  ├ physical-first.html
│  └ physical-first.pptx
│
├ themes/        # Custom Marp themes
│  └ physical-first.css
│
├ .github/
│  └ workflows/
│     └ marp-build.yml
│
└ README.md
```

---

## Authoring Slides

Edit only the Markdown files under `slides/`.

Example:

```text
slides/physical-first.md
```

---

## Exporting Slides

### Export to HTML (for GitHub Pages)

```bash
marp slides/physical-first.md --html -o dist/physical-first.html
```

### Export to PowerPoint (.pptx)

```bash
marp slides/physical-first.md --pptx
```

PowerPoint files are treated as **generated artifacts** and are not required to be committed.

---

## Publishing with GitHub Pages

If GitHub Pages is enabled, configure it to serve from:

```
/dist
```

Only HTML files are published.

---

## Recommended Workflow

```text
1. Write slides in Markdown
2. Export HTML for publishing
3. Export PPTX for presentation
4. Commit HTML only
```

This workflow keeps the repository clean and predictable.

---

## Requirements

- Node.js (LTS)
- Marp CLI

```bash
npm install -g @marp-team/marp-cli
```

---

## License

MIT License
```

