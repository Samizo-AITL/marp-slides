---
title: Architecture – Marp Slide Factory
---

# 01. Architecture – Marp Slide Factory

このドキュメントは  
**「なぜこの構成なのか」** を技術的に説明するための設計書である。

README や 00_overview.md で示した思想を  
**実装レベルに落としたもの**がここに書かれている。

---

## 🧱 全体構造（再掲）

```text
slides/*.md
   ↓
GitHub Actions
   ├─ marp --html  → dist/*.html
   ├─ marp --pptx  → dist/pptx/*.pptx
   └─ git commit (HTMLのみ)
        ↓
GitHub Pages
```

---

## 🎯 設計上の最重要原則

### 原則 1：人が編集するのは Markdown のみ

- 編集対象は **slides/*.md**
- dist/ 以下は **絶対に手で触らない**
- PPTX も HTML も「生成物」

👉  
**編集責務と生成責務を物理的に分離**するための構造。

---

### 原則 2：GitHub Pages は「静的配信装置」

GitHub Pages は以下を **一切しない**：

- Marp を実行しない
- Markdown を変換しない
- Node.js を動かさない

👉  
Pages は **dist/*.html を置く棚**にすぎない。

---

## 🔁 なぜ GitHub Actions でビルドするのか

### ローカルビルドだけではダメな理由

- 人ごとに環境が違う
- Marp のバージョン差分が出る
- 「あの人の PPTX と違う」問題が起きる

👉  
**CI で一元ビルドすることで再現性を担保**する。

---

### Actions の責務

GitHub Actions の役割は 3 つだけ：

1. Markdown を検出する
2. Marp で HTML / PPTX を生成する
3. HTML を commit する

---

## 📂 dist/ と dist/pptx/ の分離理由

| 出力 | 扱い | 理由 |
|----|----|----|
| dist/*.html | commit / Pages 配信 | 公開物 |
| dist/pptx/*.pptx | 非公開成果物 | 登壇・配布用 |

- HTML：**Web の正**
- PPTX：**人間向け成果物**

👉  
**用途が違うものはディレクトリで分ける**

---

## 🧭 index.html の意味

GitHub Pages は  
`/marp-slides/` にアクセスされたとき、

```text
dist/index.html
```

を自動的に探す。

そのため Actions で：

```bash
cp dist/physical-first.html dist/index.html
```

のように **代表スライドを index.html に割り当てる**。

---

## 🧠 なぜ HTML を commit するのか

よくある疑問：

> 「Actions が作ったファイルを commit するのは変じゃない？」

答え：**Pages の仕様上、正しい**

- Pages は「リポジトリ内容」しか見ない
- Actions のワークスペースは一時的
- commit しないと Pages から見えない

👉  
**HTML は生成物だが、配信物でもある**

---

## ⚠️ PPTX を commit しない理由（推奨）

- バイナリ差分が追えない
- 履歴が汚れる
- Pages と無関係

必要な場合のみ：

- Releases に添付
- Actions Artifacts として保存

---

## 🧩 拡張ポイント

この構成は以下に自然に拡張できる：

- カスタム theme（themes/*.css）
- PDF 出力追加
- 複数 index ページ
- Docsify / MkDocs 連携
- 他リポジトリへのテンプレ化

---

## ✅ このアーキテクチャのゴール

- 「最新版どれ？」が起きない
- HTML / PPTX が必ず一致する
- 人が増えても壊れない
- 思考と装飾を完全に分離できる

---

## 📌 まとめ（1 行）

> **このリポジトリは「スライドを作る場所」ではなく  
> 「スライドが自動で生産される工場」である**

---

_Next: 02_workflow.md_
