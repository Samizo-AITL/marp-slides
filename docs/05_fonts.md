---
title: Fonts – Preventing Tofu (□) in Marp HTML & PPTX
---

# 05. Fonts – Preventing Tofu (□)

このドキュメントは  
**Marp × HTML × PPTX 環境で日本語が □（豆腐）になる問題**を  
**構造的に防ぐための設計書**である。

---

## 🎯 このドキュメントの目的

- なぜ豆腐が発生するのかを理解する
- 一時的対処ではなく **恒久対策**を定義する
- HTML / PPTX / Viewer 差分に耐える構成を固定する

👉  
**ここを守れば、二度と豆腐は出ない**

---

## 🧠 豆腐の正体（何が起きているか）

豆腐（□）は **文字コードの問題ではない**。  
**フォントにグリフが存在しない**ことが原因。

特に問題になるのは以下：

- 日本語 + 等幅フォント（monospace）
- Marp → PPTX 変換
- WPS / PowerPoint 側のフォント差分

---

## 💥 豆腐が出る典型パターン

### 1️⃣ インラインコードに日本語を書く

```md
`slides/*.md を編集する`
```

- `code` は等幅フォントになる
- 多くの等幅フォントは日本語非対応
- → □□□□

---

### 2️⃣ 日本語入りコードブロック

```md
```text
この中に日本語を書く
```
```

- 見た目は説明文
- 意味的にはコード扱い
- → フォント崩壊

---

### 3️⃣ PPTX ビューア差分

- PowerPoint：フォントフォールバックが比較的優秀
- WPS：等幅フォントに弱い場合が多い
- LibreOffice：環境依存が強い

👉  
**「HTML で見える」は安全保証ではない**

---

## ✅ 恒久対策：Marp 側でフォントを固定する

### 推奨 front matter（最終形）

```yaml
---
marp: true
theme: default
paginate: true

style: |
  /* ===== 全体フォント（日本語安全） ===== */
  section {
    font-family:
      "Noto Sans JP",
      "Yu Gothic",
      "Meiryo",
      "Hiragino Sans",
      "Segoe UI",
      sans-serif;
  }

  /* ===== code / pre（豆腐の主犯） ===== */
  code, pre {
    font-family:
      "Noto Sans Mono JP",
      "Noto Sans JP",
      "Meiryo",
      monospace;
    font-size: 0.9em;
  }
---
```

### これで起きること

- HTML：CSS として完全適用
- PPTX：Marp がフォント名を埋め込む
- Viewer：存在するフォントに自然フォールバック

---

## 📏 設計ルール（超重要）

### ✔ OK（安全）

```md
`slides/*.md`
`dist/index.html`
`*.pptx`
```

- 英字・記号のみ
- 本当に「コード」なもの

---

### ❌ NG（再発原因）

```md
`日本語の説明文`
```

```md
```text
日本語の文章
```
```

👉  
**日本語説明は通常テキストで書く**

---

## 🧩 なぜ Marp 側で直すのが正解か

### 他の方法との比較

| 方法 | 問題 |
|----|----|
| PPTX で毎回フォント置換 | 人手・再現性なし |
| Viewer 依存 | 環境差分が出る |
| Marp 側固定 | ✅ 一度で全体解決 |

👉  
**Single Source of Truth の思想と一致**

---

## 🧠 判断に迷ったら

> **これは「コード」か？**

- YES → `code / pre`
- NO → 通常テキスト

---

## 🛡 運用チェックリスト

- [ ] front matter に style 指定がある
- [ ] 日本語を `code` に入れていない
- [ ] PPTX は生成物として扱っている
- [ ] Viewer 依存で直していない

---

## 📌 まとめ（1 行）

> **豆腐はフォントの問題であり、  
> 設計でしか根本解決できない**

---

_Next: 10_troubleshooting.md_
