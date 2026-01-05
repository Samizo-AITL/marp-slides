


# marp-slides

Markdown を唯一の正として  
HTML（GitHub Pages）と PPTX を自動生成する  
**Marp × GitHub Actions スライド工場**。

## What is this?
- slides/*.md を push すると
- HTML → GitHub Pages に公開
- PPTX → dist/pptx に生成
- フォーマット調整・最新版管理を自動化

## Philosophy
- Markdown Single Source of Truth
- Reproducibility over manual work
- Slides are *built*, not edited

## Repository Structure
(slides / dist / .github/workflows の図)

## Quick Start
1. slides/ に .md を置く
2. push
3. Pages を開く

## Docs
詳しい設計・運用ルールは 👉 docs/

---

# Marp Slides

## 📄 HTML Slides (GitHub Pages)

- Index  
  https://samizo-aitl.github.io/marp-slides/dist/index.html

- Individual slides  
  https://samizo-aitl.github.io/marp-slides/dist/01_semiconductor_case_study.html

## 📦 PPTX (Download)

- https://github.com/Samizo-AITL/marp-slides/tree/main/dist/pptx

## 🛠 Build

Slides are automatically built by GitHub Actions using Marp.


