---
title: "Udemy講座 完全運用ノート（Marp × OBS × GitHub）"
emoji: "🎥"
type: "tech"
topics: ["udemy", "marp", "obs", "markdown", "education"]
published: false
---

# Udemy講座 完全運用ノート

本資料は、Udemy 講座を  
**Markdown を唯一の正（Single Source of Truth）**として

- 資料作成
- 録画
- Udemy へのアップロード
- 公開後の更新・運用

までを **再現可能な工程**としてまとめた運用ノートである。

---

## 基本思想（最重要）

### 1. Markdown Single Source of Truth

- 人が編集するのは **Markdown のみ**
- スライド（HTML / PPTX）は **ビルド成果物**
- 動画は **成果物の記録**

---

### 2. Slides are built, not edited

- PowerPoint は編集しない
- Marp により自動生成
- CI（GitHub Actions）が正本

---

### 3. Udemy 講座は「教材資産」

- 一発撮りで終わらせない
- 更新・修正を前提とする
- 再撮影を最小化する構造を作る

---

## ディレクトリ構成（推奨）

```text
udemy-course/
├ slides/               # 編集対象（唯一の正）
│  ├ 01_intro.md
│  ├ 02_basics.md
│  └ 03_advanced.md
│
├ dist/                 # 自動生成（成果物）
│  ├ index.html
│  ├ 01_intro.html
│  ├ 02_basics.html
│  └ pptx/
│     ├ 01_intro.pptx
│     └ 02_basics.pptx
│
├ .github/workflows/
│  └ marp-build.yml
│
├ docs/
│  └ udemy_study.md     # ← 本ファイル
```

---

## スライド作成（Marp）

### Marp Front Matter（必須）

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

### 設計指針

- 1スライド ≒ 1分解説
- 箇条書きは **最大5行**
- 読ませない、**話すための補助**

---

## スライド生成（GitHub Actions）

- HTML：講義用（画面共有）
- PPTX：受講者配布用

※ `dist/` を commit するのは  
GitHub Pages が **静的ファイルを配信するだけ**だから。

---

## 録画（OBS Studio）

### OBS 基本設定

- 解像度：1920×1080
- フレームレート：30fps
- 音声：マイクのみ（PC音不要が基本）

---

### OBS シーン構成（推奨）

#### Scene 1：Slide
- ソース：画面キャプチャ or ウィンドウキャプチャ
- 表示：Marp HTML（ブラウザ全画面）

#### Scene 2：Demo（任意）
- ターミナル / エディタ
- 実演が必要な場合のみ

#### Scene 3：Break / End
- 無音・固定画面
- 編集時の区切り用

---

## 録画フロー（標準）

1. ブラウザで HTML スライドを開く
2. フルスクリーン表示
3. OBS で録画開始
4. スライドを進めながら音声解説
5. 録画停止

---

## 動画編集（最小）

- 基本：**カットのみ**
- テロップ・効果は不要
- 音ズレがなければ再編集しない

---

## Udemy へのアップロード

### セクション構成

- Section = スライド1ファイル
- Lecture = 1動画（5〜10分）

---

### 配布資料

- PPTX（Marp生成物）を添付
- ソース Markdown は **配布しない**

---

## 公開後の更新運用

### 修正が発生した場合

| 修正内容 | 対応 |
|--------|------|
| 誤字 | スライド差し替えのみ |
| 図の修正 | スライド差し替え |
| 内容追加 | 新スライド＋新動画 |
| 大幅変更 | 該当Sectionのみ再撮影 |

---

## よくある失敗と回避策

- 黒画面ばかりになる  
  → 説明はスライド、実演は補助
- PowerPoint を触りたくなる  
  → Markdown 以外編集禁止
- 更新が億劫  
  → 講座は「育てるもの」と割り切る

---

## まとめ

- Markdown を唯一の正にする
- スライドは CI で自動生成
- OBS で「映すだけ」
- Udemy 講座を **保守可能な教材資産**にする

---

## 最後に

Udemy 講座は  
**動画コンテンツではなく、設計された教材**である。

この構成を採用すると、

> 「講座を作る」から  
> **「教材を運用する」** に意識が変わる。

以上。
