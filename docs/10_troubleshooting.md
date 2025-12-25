---
title: Troubleshooting – Common Pitfalls and Fixes
---

# 10. Troubleshooting – Common Pitfalls and Fixes

このドキュメントは  
**Marp × GitHub Actions × GitHub Pages** を使った運用で  
**実際に起きがちなトラブルと、その正解ルート**をまとめたものである。

「原因 → 理由 → 正しい対処」を **構造で理解**することを目的とする。

---

## ❌ 1. Markdown を更新したのにページが変わらない

### 症状
- `slides/*.md` を編集して push した
- GitHub Pages の表示が変わらない

### 原因
- GitHub Pages は **Markdown を実行しない**
- Pages が配信するのは **commit 済みの HTML のみ**

### 正解ルート
- HTML は **GitHub Actions が生成・commit**する
- Actions が 🟢 成功しているか確認

```text
GitHub → Actions → Build Marp Slides → Success
```

---

## ❌ 2. GitHub Actions が 403 / permission denied で失敗する

### 症状
```
remote: Permission denied to github-actions[bot]
```

### 原因
- Actions の権限が **read-only**

### 正解ルート
`.github/workflows/marp-build.yml` に以下があるか確認：

```yaml
permissions:
  contents: write
```

---

## ❌ 3. `/slide-factory.html` が 404 になる

### 症状
```
https://samizo-aitl.github.io/marp-slides/slide-factory.html
→ 404
```

### 原因
- GitHub Pages の公開フォルダと URL の対応を誤解している

### 正解ルート（2択）

#### A. Pages が `/ (root)` の場合
```text
https://samizo-aitl.github.io/marp-slides/dist/slide-factory.html
```

#### B. Pages が `/dist` の場合
```text
https://samizo-aitl.github.io/marp-slides/slide-factory.html
```

👉  
**Settings → Pages の Folder 設定を必ず確認**

---

## ❌ 4. PPTX が表示されない / 見つからない

### 症状
- ブラウザで PPTX を開こうとして失敗
- Pages に PPTX が出てこない

### 原因
- GitHub Pages は **PPTX を表示しない**
- PPTX は **配布用ファイル**

### 正解ルート

- ダウンロード URL を使う：

```text
https://samizo-aitl.github.io/marp-slides/dist/pptx/slide-factory.pptx
```

- または GitHub Releases に添付

---

## ❌ 5. PPTX で日本語が □（豆腐）になる

### 症状
- HTML は正常
- PPTX で □□□ が出る

### 原因
- `code` / `pre` が等幅フォントになり
- 日本語グリフが存在しない

### 正解ルート
- Marp front matter に **日本語安全フォント指定**を入れる
- 詳細は 👉 `05_fonts.md`

---

## ❌ 6. Actions が無限ループする

### 症状
- Actions が何度も起動する

### 原因
- Actions が commit したファイルが
- `on: push` の対象に含まれている

### 正解ルート
```yaml
on:
  push:
    paths:
      - "slides/**/*.md"
```

👉  
**dist/ をトリガー対象にしない**

---

## ❌ 7. index.html が更新されない

### 症状
- トップページが古いまま

### 原因
- `Set index.html` のコピー元が固定

### 正解ルート
- 対象ファイルを見直す：

```bash
cp dist/physical-first.html dist/index.html
```

- もしくは一覧生成に切り替える

---

## 🧠 トラブル時の最終チェックリスト

- [ ] slides/*.md を編集したか
- [ ] Actions が 🟢 Success か
- [ ] HTML が commit されているか
- [ ] Pages の Folder 設定を確認したか
- [ ] URL に `dist` を含めるべきか理解しているか

---

## 📌 まとめ（1 行）

> **この構成で起きるトラブルの 9 割は  
>「GitHub Pages の仕様誤解」から始まる**

---

_End of Troubleshooting_
