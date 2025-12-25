---
title: GitHub Actions – marp-build.yml Explained
---

# 03. GitHub Actions – marp-build.yml Explained

このドキュメントは  
`.github/workflows/marp-build.yml` の **存在理由と各行の意味**を説明する。

目的は 1 つ：

> **なぜこの CI が「壊れない」のかを言語化すること**

---

## 🎯 この Actions の責務

この GitHub Actions は **多くのことをしない**。

やることは次の 3 つだけ：

1. `slides/*.md` を検出する
2. Marp で HTML / PPTX を生成する
3. HTML を commit して Pages に渡す

👉  
**それ以上のことはしない**のが設計上の重要点。

---

## 🧾 marp-build.yml（全体像）

```yaml
name: Build Marp Slides

on:
  push:
    paths:
      - "slides/**/*.md"
  workflow_dispatch:
```

### 解説
- `slides/**/*.md` が変更されたときのみ実行
- README や docs 更新では走らない
- `workflow_dispatch` で手動実行も可能

👉 **無駄に CI を回さない設計**

---

## 🔐 permissions: contents: write

```yaml
permissions:
  contents: write
```

### なぜ必要か

- Actions はデフォルトで **read-only**
- HTML を commit / push するには書き込み権限が必要

### なぜ最小権限か

- `contents: write` のみ
- secrets / token の手動指定不要
- GitHub 推奨の安全構成

---

## 🧱 job 定義

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

- Linux 環境で十分
- GUI 不要
- 再現性が高い

---

## 📥 Checkout

```yaml
- name: Checkout repository
  uses: actions/checkout@v4
```

- リポジトリを Actions 環境に展開
- shallow clone で高速

---

## 🟢 Node.js セットアップ

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: "20"
```

- Marp CLI は Node.js 依存
- LTS 固定で挙動安定

---

## 📦 Marp CLI インストール

```yaml
- name: Install Marp CLI
  run: npm install -g @marp-team/marp-cli
```

- グローバル install
- バージョン差分が出にくい
- CI 内で完結（ローカル不要）

---

## 🌐 HTML 生成ステップ

```yaml
- name: Build HTML slides
  run: |
    mkdir -p dist
    for file in slides/*.md; do
      name=$(basename "$file" .md)
      marp "$file" --html -o "dist/$name.html"
    done
```

### ポイント
- `slides/*.md` を **すべて対象**
- md 名 = html 名
- 人の判断を入れない

---

## 🏠 index.html の生成

```yaml
- name: Set index.html
  run: |
    cp dist/physical-first.html dist/index.html || true
```

- Pages の入口を固定
- 代表スライドをトップに
- `|| true` で存在しない場合も CI を止めない

👉  
**CI を落とさない設計**

---

## 📊 PPTX 生成ステップ

```yaml
- name: Build PPTX slides
  run: |
    mkdir -p dist/pptx
    for file in slides/*.md; do
      name=$(basename "$file" .md)
      marp "$file" --pptx -o "dist/pptx/$name.pptx"
    done
```

### 設計判断
- PPTX は `dist/pptx/` に隔離
- Pages と役割を分離
- HTML と混ぜない

---

## 🧾 Commit & Push

```yaml
- name: Commit generated files
  run: |
    git config user.name "github-actions"
    git config user.email "github-actions@github.com"
    git add dist
    git commit -m "Auto-generate Marp HTML & PPTX slides" || echo "No changes"
    git push
```

### なぜ commit するか

- Pages は **リポジトリ内容しか見ない**
- Actions の生成物は一時的
- commit しないと配信されない

👉  
**HTML = 生成物 かつ 配信物**

---

## ❌ やらないこと（重要）

この Actions では以下を **あえてやらない**：

- PDF 生成
- 複雑な条件分岐
- マトリクスビルド
- secrets 依存

👉  
**シンプルさが再現性を守る**

---

## 🧠 よくある誤解

### 「生成物を commit するのはアンチパターンでは？」
→ Pages を使う限り **正解**

### 「PPTX も commit すべき？」
→ 必須ではない  
→ 配布方法に応じて判断

---

## 🔧 拡張余地

この yml は自然に拡張できる：

- PDF 出力追加
- Release への PPTX 添付
- index.html 自動生成（一覧）
- theme 切り替え

---

## 📌 まとめ（1 行）

> **この GitHub Actions は  
>「賢くしないことで壊れない」CI である**

---

_Next: 05_fonts.md_
