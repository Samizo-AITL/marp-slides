---
marp: true
theme: default
paginate: true
size: 16:9
title: Marp × GitHub Actions で「スライド工場」を作る
description: Markdownを唯一の正としてHTML公開とPPTX生成を自動化する　
---

# 🏭 Marp × GitHub Actions で「スライド工場」を作る

### Markdown を唯一の正にして HTML 公開 / PPTX 生成を自動化

- Repo: https://github.com/Samizo-AITL/marp-slides  
- Pages: https://samizo-aitl.github.io/marp-slides/

---

## 🎯 今日のゴール

- `slides/*.md` を **編集して push** するだけで
  - `dist/*.html` を自動生成
  - `dist/index.html` を自動生成
  - GitHub Pages に自動デプロイ
  - `*.pptx` も自動生成（配布用）

👉 **人がやるのは Markdown だけ**

---

## 🧠 なぜ「スライド工場」なのか

- 資料は更新頻度が高い
- フォーマット調整が時間を食う
- 「最新版どれ？」が起きる

👉 **作業を手順ではなく構造に落とす**  
👉 **再現可能性（Reproducibility）を優先**

---

## 🧱 設計思想（Single Source of Truth）

- ✅ Markdown：唯一の正（設計資産）
- ✅ HTML：公開物（配信アーティファクト）
- ✅ PPTX：発表物（プレゼンアーティファクト）

❌ PowerPoint を正本にしない  
❌ GitHub Pages に変換を期待しない

---

## 📁 推奨ディレクトリ構成

```text
marp-slides/
├ slides/     # source (Markdown)
│  └ demo.md
├ dist/       # publish (HTML)
│  ├ demo.html
│  └ index.html
└ .github/workflows/
   └ marp-build.yml
```

---

## 🔁 フロー全体像

```text
ChatGPT
  ↓
slides/demo.md   ← 編集
  ↓ git push
GitHub Actions
  ↓ marp build
dist/demo.html
dist/index.html
  ↓ deploy
GitHub Pages
  ↓
/ で開ける
```

---

## ✅ Marp Front Matter（最小）

```yaml
---
marp: true
theme: default
paginate: true
size: 16:9
---
```

これがあれば **CLI / Actions と同じ挙動**になります。

---

## 🧪 デモ：V–I 制御を例に「情報粒度」を揃える

### V–I は必ずこの表記で揃える

- 電圧：$V$
- 電流：$I$
- 抵抗：$R = V/I$

---

## 🧩 「工学の階層」と「運用の階層」は同型

- 工学：Physics → Model → Control → Intelligence
- 運用：Source → Build → Artifact → Deploy

👉 どちらも **責務分離** が本質  
👉 人間が迷う箇所を「層」で潰す

---

## 🚦 よくある詰まりポイント（原因と対策）

### 1) md を更新したのにページが変わらない
- 原因：Pages は **HTML を配信**しているだけ
- 対策：Actions で **HTML を自動生成**する

### 2) PPTX と md がズレる
- 原因：PPTX を手で直す
- 対策：PPTX は **生成物**として扱う

---

## 🛡 安全運用（事故を潰す）

### 推奨：PPTX は commit しない

`.gitignore` に入れる：

```gitignore
*.pptx
```

- Repo は軽くなる
- diff ノイズが消える
- 「正本は md」の原則が守られる

---

## 🧭 index.html を自動生成する意味

- URL が 1 本で済む
- 共有・登壇・引用がラク
- 古いリンクが残らない

例：
- ✅ https://samizo-aitl.github.io/marp-slides/  
（常に最新）

---

## ✅ まとめ

- Markdown を唯一の正にする
- HTML を自動生成して公開する
- PPTX は生成物として扱う
- GitHub Actions で「工場化」する

👉 **資料作成のボトルネックは作業ではなく運用設計**

---

## 🔗 Links

- marp-slides: https://github.com/Samizo-AITL/marp-slides
- Pages: https://samizo-aitl.github.io/marp-slides/
