---
title: Overview – Marp Slide Factory
---

# 00. Overview – Marp Slide Factory

このリポジトリは  
**Markdown を唯一の正（Single Source of Truth）として**  
スライドを **自動生成・自動公開** するための  
**スライド工場（Slide Factory）** である。

---

## 🎯 目的（What problem this solves）

スライド作成では、次の問題が頻発する。

- 内容は同じなのに **HTML / PPTX / PDF がズレる**
- 「最新版どれ？」問題が必ず起きる
- フォーマット調整が思考時間を食い潰す
- 人が増えるほど **再現性が崩壊** する

👉  
**これらを「人の手順」ではなく「構造」で解決する**ことが目的。

---

## 🧠 基本思想（Core Philosophy）

### 1. Markdown Single Source of Truth
- 編集するのは **slides/*.md のみ**
- HTML / PPTX は **生成物**
- 生成物を直接編集しない

---

### 2. Slides are built, not edited
- スライドは「作る」ものではなく「**ビルドする**」もの
- GitHub Actions による自動生成が前提

---

### 3. Reproducibility over convenience
- 手動操作より **再現可能性**
- 一時的な楽さより **長期の一貫性**

---

## 🏗 全体構成（High-level Architecture）

```text
slides/*.md
   ↓ (push)
GitHub Actions
   ├─ marp --html  → dist/*.html
   ├─ marp --pptx  → dist/pptx/*.pptx
   └─ commit dist/*.html
        ↓
GitHub Pages
```

- **HTML**：公開・共有用（GitHub Pages）
- **PPTX**：登壇・配布用（ローカル利用）

---

## 📂 ディレクトリ責務

| ディレクトリ | 役割 |
|-------------|------|
| slides/ | 人が編集する唯一の場所 |
| dist/ | 自動生成された HTML |
| dist/pptx/ | 自動生成された PPTX |
| .github/workflows/ | 自動化ロジック |
| docs/ | 設計思想・運用ルール |

---

## 🌐 GitHub Pages の扱い

- GitHub Pages は **Marp を実行しない**
- Pages が見るのは **commit 済みの HTML**
- そのため HTML は **Actions で生成 → commit** する

👉  
**「なぜ Actions が commit するのか？」の答えがここにある**

---

## 📦 PPTX について

- PPTX は Pages では配信されない
- `dist/pptx/` に生成される **成果物**
- 登壇・配布・社内共有用

※ GitHub Pages の責務外

---

## 🧱 この構成で得られるもの

- 「最新版どれ？」が起きない
- HTML / PPTX の乖離が起きない
- 人が増えても壊れない
- 他リポジトリへ **そのまま転用可能**

---

## 🔗 関連ドキュメント

- 01_architecture.md：構成と責務の詳細
- 02_workflow.md：日常運用フロー
- 03_github-actions.md：CI 定義の解説
- 05_fonts.md：日本語豆腐対策（重要）

---

## ✅ このドキュメントの役割

この `00_overview.md` は  
**「このリポジトリは何か」を一文で説明するための固定点**である。

ここに書いてある思想を変えるときは  
**構成そのものを変える覚悟が必要**。

---

_End of Overview_
