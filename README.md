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

## 📄 Generated HTML Slides (GitHub Pages)

以下は Marp から自動生成された HTML スライドです。  
ブラウザ上で日本語表示を確認できます。

- ▶ Index  
  https://samizo-aitl.github.io/marp-slides/index.html

- ▶ 01 Semiconductor Case Study  
  https://samizo-aitl.github.io/marp-slides/01_semiconductor_case_study.html

- ▶ 02 PZT Thin-Film Actuator Case Study  
  https://samizo-aitl.github.io/marp-slides/02_pzt_thinfilm_actuator_case_study.html

- ▶ 03 Printhead Manufacturing Case Study  
  https://samizo-aitl.github.io/marp-slides/03_printhead_manufacturing_case_study.html

- ▶ 04 BOM Workflow Case Study  
  https://samizo-aitl.github.io/marp-slides/04_bom_workflow_case_study.html

> Note  
> - HTML では日本語は正常表示されます。  
> - PPTX で日本語が表示されない場合は、フォント未埋め込みが原因です。

