---
title: "marp-slides"
description: "marp-slides"
---

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

---

## 👤 Author

| 📌 Item | Details |
|--------|---------|
| **Name** | Shinichi Samizo |
| **Expertise** | Semiconductor devices (logic, memory, high-voltage mixed-signal)<br>Thin-film piezo actuators for inkjet systems<br>PrecisionCore printhead productization, BOM management, ISO training |
| **GitHub** | [![GitHub](https://img.shields.io/badge/GitHub-Samizo--AITL-blue?style=for-the-badge&logo=github)](https://github.com/Samizo-AITL) |

---

## 📄 License

[![Hybrid License](https://img.shields.io/badge/license-Hybrid-blueviolet)](https://samizo-aitl.github.io/mems-ana//#-license)

| 📌 Item | License | Description |
|--------|---------|-------------|
| **Source Code** | [**MIT License**](https://opensource.org/licenses/MIT) | Free to use, modify, and redistribute |
| **Text Materials** | [**CC BY 4.0**](https://creativecommons.org/licenses/by/4.0/) or [**CC BY-SA 4.0**](https://creativecommons.org/licenses/by-sa/4.0/) | Attribution required; share-alike applies for BY-SA |
| **Figures & Diagrams** | [**CC BY-NC 4.0**](https://creativecommons.org/licenses/by-nc/4.0/) | Non-commercial use only |
| **External References** | Follow the original license | Cite the original source properly |



