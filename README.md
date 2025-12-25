# marp-slides

This repository is a **Markdown-first presentation workspace** built on **Marp**.

Slides are authored in Markdown and exported into purpose-specific artifacts:

- **HTML** for publishing and sharing via GitHub Pages  
- **PowerPoint (.pptx)** for talks and offline presentations  

The core goal is to **separate thinking from formatting** and make slide creation
**reproducible, fast, and version-controlled**.

---

## 📊 Published Slides

- **Physical-First Engineering & Intelligent Control Architecture**  
  https://samizo-aitl.github.io/marp-slides/dist/physical-first.html

> GitHub Pages serves **pre-generated HTML only**.  
> Markdown files are never rendered directly by GitHub Pages.

---

## Philosophy

- **Markdown is the single source of truth**
- **HTML is the published artifact**
- **PowerPoint is the presentation artifact**
- **GitHub Pages does not execute Marp**

This strict separation prevents common failures such as:

- “Markdown was updated but the page did not change”
- “PowerPoint slides are out of sync with the source”

---

## Repository Structure

```text
marp-slides/
├ slides/        # Marp source files (Markdown)
│  └ physical-first.md
│
├ dist/          # Generated artifacts (published)
│  └ physical-first.html
│
├ themes/        # Custom Marp themes (optional / future use)
│
└ README.md
```

---

## Authoring Slides

Edit **only** Markdown files under `slides/`.

Example:

```text
slides/physical-first.md
```

Each file must include a Marp front matter header:

```yaml
---
marp: true
theme: default
paginate: true
size: 16:9
---
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

- HTML is a **published artifact** and should be committed
- PPTX is a **presentation artifact** and does not need to be committed

---

## Publishing with GitHub Pages

GitHub Pages is configured to deploy from the repository root.

Slides are accessed via **direct HTML paths**, for example:

```text
/dist/physical-first.html
```

This avoids relying on GitHub Pages folder heuristics and keeps deployment explicit
and predictable.

---

## Recommended Workflow

```text
1. Write slides in Markdown (slides/)
2. Generate HTML for publishing (dist/)
3. Generate PPTX for presentation
4. Commit Markdown + HTML only
```

This keeps the repository clean and ensures reproducibility.

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
