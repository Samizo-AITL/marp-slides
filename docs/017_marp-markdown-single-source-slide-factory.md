---
title: "【Marp】Markdownを唯一の正にしてスライドを自動生成・公開する完全運用"
emoji: "🏭"
type: "tech"
topics: ["marp", "githubactions", "githubpages", "markdown", "presentation"]
published: true
---

## はじめに

「スライドを Markdown で書ける」のは、もはや珍しくありません。  
しかし現実には、こんな問題が頻発します。

- Markdown を直したのに **公開ページが変わらない**
- HTML と PowerPoint の内容が **ズレる**
- 「最新版どれ？」問題が必ず起きる
- フォントや日本語が **□（豆腐）** になる

この記事では、これらを  
**手順ではなく“構造”で解決する運用**を紹介します。

結論から言うと：

> **Markdown を唯一の正（Single Source of Truth）として  
> スライドを“作る”のではなく“ビルドする”**

という考え方に切り替えます。

---

## ゴール

この記事で作るのは、次のような「スライド工場」です。

- `slides/*.md` を編集して push するだけ
- GitHub Actions が自動で
  - HTML を生成して GitHub Pages に公開
  - PPTX を生成して配布用に保存
- 人は **PowerPoint を一切触らない**

---

## 基本思想（重要）

この運用は、次の 3 原則で成り立っています。

### 1. Markdown Single Source of Truth
- 編集するのは **Markdown だけ**
- HTML / PPTX は **生成物**

### 2. Slides are built, not edited
- スライドは「作業」ではなく「ビルド成果物」
- CI（GitHub Actions）が正本

### 3. GitHub Pages は“棚”
- Pages は **HTML を配信するだけ**
- Marp は実行しない

---

## ディレクトリ構成

```text
marp-slides/
├ slides/           # 人が編集する唯一の場所
│  └ slide-factory.md
│
├ dist/             # 自動生成（公開）
│  ├ index.html
│  ├ slide-factory.html
│  └ pptx/
│     └ slide-factory.pptx
│
├ .github/workflows/
│  └ marp-build.yml
│
└ docs/             # 設計・運用ドキュメント
```

---

## Marp 用 Markdown（豆腐対策込み）

PPTX で □（豆腐）が出る最大の原因は  
**インラインコードが等幅フォントになること**です。

以下の front matter を **必ず入れます**。

```yaml
---
marp: true
theme: default
paginate: true
size: 16:9

style: |
  section {
    font-family:
      "Noto Sans JP",
      "Yu Gothic",
      "Meiryo",
      "Segoe UI",
      sans-serif;
  }

  code, pre {
    font-family:
      "Noto Sans Mono JP",
      "Noto Sans JP",
      "Meiryo",
      monospace;
  }
---
```

これで HTML / PPTX / PowerPoint / WPS すべてで  
日本語が安全に表示されます。

---

## GitHub Actions（完成版）

```yaml
name: Build Marp Slides

permissions:
  contents: write

on:
  push:
    paths:
      - "slides/**/*.md"
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: "20"

      - run: npm install -g @marp-team/marp-cli

      - name: Build HTML
        run: |
          mkdir -p dist
          for file in slides/*.md; do
            name=$(basename "$file" .md)
            marp "$file" --html -o "dist/$name.html"
          done

      - name: Set index.html
        run: |
          cp dist/slide-factory.html dist/index.html || true

      - name: Build PPTX
        run: |
          mkdir -p dist/pptx
          for file in slides/*.md; do
            name=$(basename "$file" .md)
            marp "$file" --pptx -o "dist/pptx/$name.pptx"
          done

      - name: Commit
        run: |
          git config user.name "github-actions"
          git config user.email "github-actions@github.com"
          git add dist
          git commit -m "Auto-generate Marp slides" || echo "No changes"
          git push
```

---

## 公開と配布

### HTML（閲覧）
```
https://<username>.github.io/<repo>/dist/slide-factory.html
```

### PPTX（ダウンロード）
```
https://<username>.github.io/<repo>/dist/pptx/slide-factory.pptx
```

※ GitHub Pages は PPTX を「表示」しません  
※ **ダウンロード用成果物**として扱います

---

## よくある詰まりポイント

- Markdown を直したのに反映されない  
  → **Actions が成功しているか確認**
- 403 エラー  
  → `permissions: contents: write` を忘れている
- PPTX が見えない  
  → Pages は配信しない（仕様）
- 日本語が □  
  → フォント設計の問題（この記事の対策で解決）

---

## まとめ

- Markdown を唯一の正にする
- スライドは CI で自動生成する
- HTML は公開物、PPTX は配布物
- 人は PowerPoint を触らない

この構成にすると：

> **「スライド作成」が  
> 再現可能なビルド工程になる**

---

## おわりに

この記事で紹介した構成は、  
個人利用だけでなく **教育・研究・社内資料・OSS** にもそのまま使えます。

一度作ってしまえば、  
**もう「資料作成に戻る」ことはありません。**
