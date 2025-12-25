---
title: Workflow – Daily Operation of Marp Slide Factory
---

# 02. Workflow – Daily Operation

このドキュメントは  
**このリポジトリを日常的にどう使うか**を定義する運用仕様書である。

設計思想やアーキテクチャを知らなくても  
**この手順だけ守れば壊れない**ことを目的とする。

---

## 🎯 想定ユーザー

- 自分（設計者）
- 将来の自分
- 他人（引き継ぎ・共同作業）

👉  
「考えなくても同じ結果になる」ことが最優先。

---

## 🗂 編集してよい場所 / ダメな場所

### ✅ 編集してよい

```text
slides/*.md
themes/*.css
docs/*.md
README.md
```

### ❌ 編集してはいけない

```text
dist/
dist/pptx/
.github/workflows/
```

👉  
**dist 以下は生成物**  
手で触った時点で破綻する。

---

## ✍️ 日常の作業フロー（最短）

### ① スライドを書く

```text
slides/slide-factory.md
```

- Markdown だけを書く
- フォーマットは考えない
- 改ページは `---` のみ

---

### ② commit & push する

```bash
git add slides/slide-factory.md
git commit -m "Update slide-factory slides"
git push
```

👉  
**これだけで OK**

---

### ③ 自動で起きること（人は何もしない）

GitHub Actions が以下を自動実行：

1. Marp CLI をインストール
2. slides/*.md を検出
3. HTML を dist/*.html に生成
4. PPTX を dist/pptx/*.pptx に生成
5. HTML を commit
6. GitHub Pages に反映

---

### ④ 確認する場所

| 種類 | 場所 |
|----|----|
| Web公開 | https://samizo-aitl.github.io/marp-slides/ |
| 個別HTML | `/dist/xxx.html` |
| PPTX | `dist/pptx/*.pptx`（DL用） |
| ログ | GitHub Actions → Runs |

---

## 🔁 修正ループ

```text
修正したい
 ↓
slides/*.md を編集
 ↓
push
 ↓
Pages が更新される
```

👉  
**HTML や PPTX を直接触る工程は存在しない**

---

## 🚫 やってはいけないこと一覧

### ❌ PPTX を修正して再保存

- 次の push で上書きされる
- 正のソースではない

### ❌ dist/*.html を手編集

- CI が次回破壊する
- Pages と乖離する

### ❌ Actions の yml を場当たり修正

- 全体設計を壊す
- 修正は docs/ で合意してから

---

## 🧠 判断に迷ったらこのルール

> **「これは Markdown か？」**

- YES → OK
- NO → 触るな

---

## 🧩 よくあるケース別対応

### スライドを追加したい

```text
slides/new-topic.md を作る
```

- index.html にしたければ Actions 側で指定変更

---

### PPTX だけ欲しい

- Actions 実行後に `dist/pptx/*.pptx` をダウンロード
- commit 不要

---

### 公開したくないスライド

- 別ブランチで管理
- もしくは別リポジトリ

---

## ✅ この運用のゴール

- 毎回同じ結果が出る
- 「誰がやっても」同じ
- 手作業が入り込まない
- 記憶に頼らない

---

## 📌 まとめ（1 行）

> **このリポジトリでは  
>「作業」ではなく「push」だけが存在する**

---

_Next: 03_github-actions.md_
