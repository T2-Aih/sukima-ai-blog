# Icon Style Guide - Sukima AI Journal

## スタイル定義

Humationを参考にしつつ、オリジナリティを出した独自スタイル。

### カラー
- **メイン**: チャコールグレー (#2D2D2D) ※Humation風
- **背景**: 白 (#FFFFFF)
- **アクセント**: 薄グレー (#F5F5F5) ※塗りに使用可

### 線のスタイル
- **太さ**: 均一で整った線（2-3px相当）
- **角**: 丸みを帯びた角（rounded corners）
- **形**: 幾何学的だが親しみやすい

### モチーフ
AI/テック系に特化。人物は最小限。

### サイズ
- 生成: 1024x1024px (1K)
- 使用: 80-100px で表示

---

## アイコン一覧

| ID | 名前 | 説明 | ファイル |
|----|------|------|----------|
| 01 | smartphone | スマートフォン | icon-smartphone.png |
| 02 | ai-brain | AI/脳 | icon-ai-brain.png |
| 03 | settings | 設定/歯車 | icon-settings.png |
| 04 | lightbulb | ひらめき/電球 | icon-lightbulb.png |
| 05 | code | コード/ターミナル | icon-code.png |
| 06 | flow | フロー/矢印 | icon-flow.png |
| 07 | checklist | チェックリスト | icon-checklist.png |
| 08 | clock | 時計/スキマ時間 | icon-clock.png |
| 09 | connect | 接続/リンク | icon-connect.png |
| 10 | rocket | ロケット/スタート | icon-rocket.png |
| 11 | person-man | 男性 | icon-person-man.png |
| 12 | person-woman | 女性 | icon-person-woman.png |
| 13 | person-child | 子供 | icon-person-child.png |

---

---

## 固有名詞・ブランドロゴの扱い

**ルール**: 記事に固有名詞（サービス名、ツール名）が出てきた場合、公式ロゴを使用して画像を生成する。

### ロゴ収集手順
1. 公式サイトからロゴをダウンロード
2. `src/assets/logos/` に保存
3. nano-banana-proの `-i` オプションで参照して合成

### ロゴ保存先
```
src/assets/logos/
├── openai.png      # OpenAI / ChatGPT
├── anthropic.png   # Anthropic / Claude
├── gemini.png      # Google Gemini
├── github.png      # GitHub / Copilot
├── perplexity.png  # Perplexity AI
└── ...
```

### 注意
- 商標利用規約を確認すること
- 改変が禁止されているロゴは改変しない
- ロゴ単体ではなく、図解の一部として使用

---

## プロンプトテンプレート

```
Simple flat icon illustration: [モチーフ], 
minimalist geometric style with rounded corners, 
single color (#6366F1 indigo) on white background, 
clean uniform line weight, 
friendly and modern, 
no text, 
centered composition,
suitable for 80px display
```
